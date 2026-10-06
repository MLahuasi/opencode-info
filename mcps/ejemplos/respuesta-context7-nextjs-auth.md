# Ejemplo de respuesta de Context7 sobre autenticación en Next.js

> **Ejemplo de respuesta**
>
> Este documento conserva una respuesta generada por OpenCode después de consultar Context7. Sirve para mostrar el tipo de resultado que puede producir el MCP; no constituye una guía completa de autenticación para Next.js.

## Consulta

```text
¿Cómo se deben proteger las rutas en Next.js? Usa Context7 para consultar la documentación actual.
```

## Herramientas invocadas

```text
→Skill "context7-mcp"
⚙ context7_resolve-library-id [libraryName=Next.js, query=Current Next.js documentation for protecting routes with authentication, authorization, middleware or proxy, server components, and redirects]
⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official authentication and authorization guidance for protecting routes in the App Router, including Server Components, redirect, cookies, and checking permissions close to the data source]
⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official Next.js Proxy documentation for redirecting unauthenticated users, matcher configuration, and security limitations]
⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official Next.js guidance for protecting Route Handlers and Server Actions, validating sessions and authorization on the server]
```

## Respuesta resumida

En Next.js, la autorización debe comprobarse en el servidor, no únicamente ocultando componentes en el cliente. La respuesta organiza la protección en estas capas:

1. **Autenticación:** comprobar si existe una sesión válida.
2. **Autorización:** comprobar permisos para cada recurso o acción.
3. **Proxy:** hacer comprobaciones optimistas y redirecciones tempranas.
4. **Data Access Layer (DAL):** verificar sesión y permisos cerca del acceso a los datos.

Los siguientes fragmentos son patrones de referencia. `auth`, `decrypt` y los tipos de sesión dependen de la solución de autenticación del proyecto; no son módulos incluidos automáticamente por Next.js.

### Verificar una sesión desde una DAL

```ts
// app/lib/dal.ts

import { cache } from "react";
import { auth } from "@/lib/auth";

export const getSession = cache(async () => {
  const session = await auth();

  return session?.user ? session : null;
});
```

Una página puede redirigir al usuario si no existe una sesión:

```ts
// app/dashboard/page.tsx

import { redirect } from "next/navigation";
import { getSession } from "@/app/lib/dal";

export default async function DashboardPage() {
  const session = await getSession();

  if (!session) {
    redirect("/login");
  }

  return <h1>Hola, {session.user.name}</h1>;
}
```

### Usar `proxy.ts` para comprobaciones optimistas

En las versiones actuales, Next.js utiliza `proxy.ts` para redirecciones optimistas:

```ts
// proxy.ts

import { NextRequest, NextResponse } from "next/server";
import { decrypt } from "@/app/lib/session";

export async function proxy(request: NextRequest) {
  const sessionCookie = request.cookies.get("session")?.value;
  const session = await decrypt(sessionCookie);

  if (!session?.userId) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ["/dashboard/:path*", "/admin/:path*"],
};
```

El `proxy` no debe ser la única defensa. Úsalo para comprobaciones rápidas y completa la autorización cerca de los datos. La [guía de autenticación de Next.js](https://nextjs.org/docs/app/guides/authentication) recomienda una DAL para centralizar esas comprobaciones.

### Proteger Route Handlers

```ts
// app/api/admin/users/route.ts

import { getSession } from "@/app/lib/dal";

export async function GET() {
  const session = await getSession();

  if (!session) {
    return new Response(null, { status: 401 });
  }

  if (session.user.role !== "admin") {
    return new Response(null, { status: 403 });
  }

  return Response.json({ users: [] });
}
```

El handler devuelve `401` si no hay sesión y `403` si el usuario no tiene permisos.

### Proteger Server Actions

Una página protegida no protege automáticamente sus Server Actions:

```ts
"use server";

import { getSession } from "@/app/lib/dal";

export async function deleteUser(userId: string) {
  const session = await getSession();

  if (!session) {
    throw new Error("Unauthorized");
  }

  if (session.user.role !== "admin") {
    throw new Error("Forbidden");
  }

  // Eliminar solamente después de verificar sesión y permisos.
}
```

### Reglas de seguridad mostradas por la respuesta

- No confiar en `useEffect`, `localStorage` ni guards únicamente del cliente.
- No proteger solamente desde un `layout`.
- Validar autorización también por objeto.
- Usar cookies `HttpOnly`, `Secure` y `SameSite` para la sesión.
- Comprobar sesión y permisos en páginas, Route Handlers y Server Actions por separado.
- Usar una librería de autenticación establecida.

Referencias: [Authentication](https://nextjs.org/docs/app/guides/authentication), [Data Security](https://nextjs.org/docs/app/guides/data-security) y [Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy).

[← Volver a Context7](../03-context7.md) | [Temario](../../10-opencode-mcp.md)
