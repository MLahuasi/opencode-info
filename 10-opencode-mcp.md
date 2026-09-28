# MCPs y Agentes Personalizados

## Configuración APP

- Crear app

```bash
npx create-next-app@latest open-daycare
# Dar clic en opciones recomendadas
```

- Una vez instalada se levanta la app ejecutando:

```bash
    npm run dev
```

- Se crea la aplicación con los archivos:

  > - [`AGENTS.md`](../open-daycare/AGENTS.md): En esta version nest.js crea un conjunto de archivos (cambian continuamente) para ayudar en la generación de código fuente con agentes de AI. **NOTA**: en esta versión se encuentran en `node_modules/next/dist/docs/`

  > ![](./assets/12-open-care-next-ia-docs.png)

  > - [`CALUDE.md`](../open-daycare/CLAUDE.md): Hace referencia a `@AGENTS.md` para que si se genera código con `Claude Code` se analize primero la información de `AGENTS.md`.

- Crear un directorio [`references`](../open-daycare/references/) y colocar las plantillas `html` e `imágenes` generadas mediante IA. Se pone a consideración las siguientes herramientas:

> - **`Herramientas para crear Pantallas HTLM` (pagadas)**
>   > - [**Claude Design**](https://claude.ai/design)
>   > - [**V0**](https://v0.app/)
>   > - [**Lovable**](https://lovable.dev/)
>   > - [**Google Stitch**](https://stitch.withgoogle.com/)
>   > - [**Bolt**](https://bolt.new/)

## MCPs

Los MCPs aumentan la cuota por lo que deben ser muy puntuales para su uso. Existen varios.

### [Playwright](https://playwright.dev/)

Le da a los agentes un navegador web en el cual los agentes pueden interactuar. Sirve para sitios web.

#### Configurar en OpenCode

- Ir a [Documentación](https://playwright.dev/docs/getting-started-mcp) y copiar:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

- Ingresar desde la terminal al proyecto y ejecutar `opencode`
- En modo `Build` solictar:

```text
Instala el mcp Playwright localmente
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

- Con esta orden `OpenCode ` puede a solicitar permisos para instalar y configurar en el proyecto. **NOTA**: En esta version `OpenCode` generó el archivo [opencode.json](../open-daycare/opencode.json):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": ["npx", "@playwright/mcp@latest"],
      "enabled": true
    }
  }
}
```

**NOTA**: En el caso de que se presenten errores porque `OpenCode` no reconoce correctamente a `NVM`

```json
// Version Windows
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": [
        // <NVM_HOME>\<versión-node>\npx.cmd
        "C:\\Users\\Developer\\AppData\\Roaming\\nvm\\v22.23.2\\npx.cmd",
        "-y",
        "@playwright/mcp@latest"
      ],
      "enabled": true,
      "environment": {
        // <NVM_HOME>\<versión-node>;<PATH-del-sistema>
        "PATH": "C:\\Users\\Developer\\AppData\\Roaming\\nvm\\v22.23.2;C:\\Windows\\System32;C:\\Windows"
      }
    }
  }
}
```

o

```json
// Version Linux/macOS
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": [
        // <NVM_HOME>/versions/node/<versión-node>/bin/npx
        "/Users/strider/.nvm/versions/node/v22.20.0/bin/npx",
        "-y",
        "@playwright/mcp@latest"
      ],
      "enabled": true,
      "environment": {
        // <NVM_HOME>/versions/node/<versión-node>/bin:<PATH-del-sistema>
        "PATH": "/Users/strider/.nvm/versions/node/v22.20.0/bin:/usr/local/bin:/usr/bin:/bin"
      }
    }
  }
}
```

- Reiniciar la sesión
- Abrir mcps con el comando `/mcps`

  ![](./assets/13-opencode-mcp-playwright-config.png)

- Una vez configurado el `MCP` en modo `Build` solicitar:

```text
Utiliza el MCP Playwright y revisa el home (/)
```

- `OpenCode` realiza lo siguiente:

  > - Ejecuta la aplicación desde una `consola`
  > - Abre un navegador con la aplicación corriendo
  > - crea el directorio `.playwright-mcp`. **NOTA**: Este directorio no debería pasar al repositorio de `GitHub`

- Se puede solictar solicitar en modo `Build`

```text
captura un screenshot
```

> - Home Desktop:

> ![](./assets/14-opencode-mcp-local-screenshot-home-desktop.png)
>
> - Home Mobile:

> ![](./assets/15-opencode-mcp-local-screenshot-home-mobile.png)
>
> - Home:

> ![](./assets/16-opencode-mcp-local-screenshot-home-screenshot.png)

**NOTA**:

Para que el agente guarde los `Screenshots` en un directorio se puede modificar [AGENTS.md](../open-daycare/AGENTS.md) con una instrucción semenjante a:

```md
## MCPs

- Cualquier Screenshot o archivo relacionado a `Playwright` se deben almacenar en `.playwright-mcp`
```

---

### [Context7](https://context7.com/)

`Context7` sirve para darle a un asistente de IA documentación `actualizada y específica de la versión` de una librería o framework mientras estás programando. Su objetivo principal es reducir respuestas basadas en documentación antigua o APIs inventadas.

Es de pago, se debe crear una cuenta, tiene una capa gratuita.

#### Configurar en OpenCode

- Ir a [Documentación](https://context7.com/install):
- Ejecutar

```bash
npx ctx7 setup
```

- Seleccionar `MCP server - Agents calls Context7 tools via MCP protocol to retrieve up-to-date library docs`
- Seleccionar `OpenCode`
  > - Se crea una `API KEY` en un archivo `opencode.jsonc`
  > - Se añade una regla en `.config\opencode\AGENTS.md`. **NOTA** : Depende del lugar en donde se cnfiguró `OpenCode`
  > - Se crea `.agents\skills\context7-mcp\SKILL.md` en la carpeta de `user`
- Ingresar desde la `consola` al directorio de la aplicación ejecutar `opencode`
- En modo `Build` ejecutar el comando `/mcps` y verificar que se encuentre conectado `Context7`.

  ![](./assets/17-opencode-mcp-context7-config.png)

- Que hace [Context7](https://context7.com/docs/overview)

`Context7` proporciona al `LLM` **documentación actualizada y específica de la versión de las librerías del proyecto**.

Sin `Context7`, el modelo puede usar información antigua, APIs inexistentes o ejemplos de versiones desactualizadas. Con Context7 MCP, el asistente consulta documentación y ejemplos actuales directamente desde la fuente, mejorando la compatibilidad del código generado.

Para usarlo, se puede añadir `use context7` al final del prompt.

- Ejecutar en modo `Build` en `OpenCode`

```text
Como se deben proteger rutas en Next.js usa context7
```

- `OpenCode` responde:

Consultaré la documentación actual de Next.js mediante Context7 y resumiré los patrones recomendados para proteger rutas, diferenciando protección en servidor, middleware/proxy y autorización por recurso.

```text
→Skill "context7-mcp"
⚙ context7_resolve-library-id [libraryName=Next.js, query=Current Next.js documentation for protecting routes with authentication, authorization, middleware or proxy, server components, and redirects]

⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official authentication and authorization guidance for protecting routes in the App Router, including Server Components, redirect, cookies, and checking permissions close to the data source]

⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official Next.js middleware or proxy documentation for redirecting unauthenticated users and matcher configuration, including security limitations]

⚙ context7_query-docs [libraryId=/vercel/next.js, query=Official Next.js guidance for protecting Route Handlers and Server Actions, validating sessions and authorization on the server]
```

`+ Thought: Clarifying Next.js Proxy and Authentication Concepts · 3.8s`

En Next.js, la protección debe hacerse `en el servidor`, no únicamente ocultando componentes en el cliente. La estrategia recomendada es combinar:

> 1. `Autenticación`: comprobar si existe una sesión válida.
> 2. `Autorización`: comprobar roles/permisos para cada recurso o acción.
> 3. `Proxy`: redirección rápida y optimista antes de renderizar.
> 4. `Data Access` Layer: verificación definitiva cerca de los datos.

1. **Verificación centralizada de sesión**

```ts
// app/lib/dal.ts

import { cache } from "react";
import { redirect } from "next/navigation";
import { auth } from "@/lib/auth";

export const verifySession = cache(async () => {
  const session = await auth();

  if (!session?.user) {
    redirect("/login");
  }

  return session;
});
```

2. **Proteger una página**

```ts
// app/dashboard/page.tsx

import { verifySession } from "@/app/lib/dal";

export default async function DashboardPage() {
  const session = await verifySession();

  return <h1>Hola, {session.user.name}</h1>;
}
```

3. **Proteger rutas mediante `proxy.ts`**

En las versiones actuales, Next.js utiliza proxy.ts para redirecciones optimistas:

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

El `proxy` no debe ser la única defensa. La documentación recomienda evitar consultas a la base de datos allí y usarlo para redirecciones rápidas.

4. **Proteger Route Handlers**

```ts
// app/api/admin/users/route.ts

import { verifySession } from "@/app/lib/dal";

export async function GET() {
  const session = await verifySession();

  if (session.user.role !== "admin") {
    return new Response(null, { status: 403 });
  }

  return Response.json({ users: [] });
}
```

Usa:

- `401` cuando el usuario no está autenticado.
- `403` cuando está autenticado pero no tiene permisos.

5. **Proteger Server Actions**

Una página protegida no protege automáticamente sus Server Actions:

```ts
"use server";

import { verifySession } from "@/app/lib/dal";

export async function deleteUser(userId: string) {
  const session = await verifySession();

  if (session.user.role !== "admin") {
    throw new Error("Forbidden");
  }

  // Eliminar solamente después de verificar sesión y permisos.
}
```

**Reglas importantes**

- No confíes en `useEffect`, `localStorage` ni guards únicamente del cliente.
- No protejas solamente desde un `layout`; los layouts no necesariamente se vuelven a ejecutar durante la navegación.
- Valida autorización también por objeto: un usuario autenticado no debe poder consultar cualquier `/users/[id]`.
- Usa cookies `HttpOnly`, `Secure` y `SameSite` para la sesión.
- Comprueba sesión y permisos en páginas, Route Handlers y Server Actions por separado.
- Usa una librería de autenticación establecida para crear y validar sesiones; Next.js no proporciona por sí mismo un sistema completo de autenticación.

Fuentes oficiales consultadas: `Authentication` ([https://nextjs.org/docs/app/guides/authentication](https://nextjs.org/docs/app/guides/authentication)), `Data Security` ([https://nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)) y `Proxy` ([https://nextjs.org/docs/app/api-reference/file-conventions/proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)).

---

## Uso de Skills

Para este laboratorio se utilizan las Skills desarrolladas por [Klerith](https://github.com/Klerith/fernando-skills/tree/main), diseñadas para trabajar con un flujo basado en `specs`.

### Instalar las Skills

Ejecutar:

```bash
# Instala las Skills disponibles desde el repositorio indicado.
npx skills add klerith/fernando-skills
```

Se generará una estructura similar a:

```text
.agents/
└── skills/
    ├── spec/
    │   ├── SKILL.md
    │   └── template.md
    │
    └── spec-impl/
        └── SKILL.md
```

**NOTA**:

Modificar [AGENTS.md](../open-daycare/AGENTS.md) con la funcionalidad de `Context7`:

```md
<!-- Agrega a la sección -->

## MCPs

- Usa `Context7` para obtener documentación actualizada del `framework`.
```

---

## Configurar Aplicación

### Iniciar App

- En [AGENTS.md](../open-daycare/AGENTS.md) configurar reglas por ejemplo:

```md
## Workflow (MCPs)

- Para funcionalidades grandes, cargar `spec`; para implementar una spec aprobada, cargar `spec-impl` y respetar sus pausas de revisión.
- Validador de aceptación del proyecto: agente `@spec-acceptance-validator` (`.opencode/agent/spec-acceptance-validator.md`) y comando `/spec-acceptance-validator <spec>` (`.opencode/command/spec-acceptance-validator.md`). Corrige incumplimientos, marca solo criterios verificados y usa Context7 y Playwright cuando aplica.
- Los nombres de specs se relacionan con el módulo, no con una captura o prototipo.

## Arquitectura

- Usar feature-first: `components/ui` para UI genérica, `shared` para código transversal y `features/<domain>` para cada dominio.
- Una feature puede contener `components`, `data`, `types`, `schemas`, `actions`, `services` y `utils`.
- Las features no dependen de internals de otras features. Exponer su API pública mediante `index.ts` y mover lo común a `shared`.
- Los archivos especiales de App Router mantienen su convención de Next.js; la organización interna no crea rutas sin `page` o `route`.

## Código

- Nombres de código y archivos en inglés. Mantener responsabilidades claras, bajo acoplamiento y evitar abstracciones o cambios no solicitados.
- Separar presentación, datos, validación, estado y lógica de negocio cuando mezclarlo reduzca claridad.

### Documentación

- APIs exportadas y componentes reutilizables DEBEN tener JSDoc.
- El JSDoc de funciones incluye descripción, `@param`, cada prop propia recibida y `@returns`.
- Si las props extienden atributos nativos, indicarlo. Usar `//` solo para decisiones o lógica no evidente.

### React

- Componentes reutilizables DEBEN aceptar y combinar `className?: string`.
- Eventos configurables se reciben por props; no agregar handlers, enlaces ni navegación ficticios.
- Mantener Server Components por defecto. Usar `"use client"` solo para estado, eventos, hooks o APIs del navegador.
- Los elementos interactivos incluyen los estados visuales aplicables: `hover`, `focus-visible`, `disabled`, `loading` o `cursor-pointer`.

### Datos y configuración

- Componentes NO DEBEN contener datos mock o de negocio. Ubicarlos tipados en `features/<feature>/data` o recibirlos por props.
- Datos mock incluyen nombres, fechas, cantidades, publicaciones, etiquetas variables y opciones de navegación.
- Configuración compartida usa constantes; configuración de entorno usa variables de entorno. Se permiten literales técnicos, SVG y copy propio de componentes genéricos.

### Estilos

- Usar Tailwind para la mayoría de estilos y layout, conforme a la guía local de Next.js.
- Usar CSS Modules colocados junto al componente cuando estilos o variantes complejos no sean claros con utilities.
- `globals.css` se reserva para Tailwind, reset, fuentes y tokens globales.
- Colores, sombras y gradientes compartidos usan tokens semánticos. No usar estilos inline salvo valores calculados dinámicamente.
```

- Ingresar al directorio del proyecto y ejecutar `opencode`
- `Opcional` en modo `Plan` solicitar que analice el contexto para la ejecución de `/init` ya que `AGENTS.md` tiene contenido y se debe respetar, tambien que considere las `skills` instaladas.

```text
Analiza el contexto del proyecto antes de ejecutar /init. @AGENTS.md ya contiene instrucciones que deben preservarse. Respeta su contenido, estructura, referencias a documentación y las skills disponibles en .agents. Completa únicamente la información necesaria para inicializar el proyecto, sin sobrescribir, duplicar ni eliminar reglas existentes.
```

- En modo `Build` ejecutar el comando `/init`. **NOTA**: Esto debe modificar el archivo [`AGENTS`](../open-daycare/AGENTS.md)
- Es recomendable hacer un commit a git.

```bash
git add .
git commit -m "DayCare - OpenCode Config"
```

---

### Diseño Aplicación

#### [Home](./opencode-daycare/home.md)

#### [Kids](./opencode-daycare/kids.md)

### [Login y Activate](./opencode-daycare/login-activate.md)

### [Add y Edit Kid](./opencode-daycare/add-edit-kid.md)

### [Instalar Dependencia Externa - Email](./opencode-daycare/email.md)

### [Reorganizar Arquitectura](./opencode-daycare/organice-architecture.md)

### [Activar Cuenta](./opencode-daycare/activate-account.md)

### [Post](./opencode-daycare/create-update-post.md)
