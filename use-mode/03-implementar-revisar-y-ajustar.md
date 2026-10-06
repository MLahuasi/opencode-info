# Implementar, revisar y ajustar

> **Ejemplo:** este documento muestra un flujo de referencia. Los comandos, archivos y mensajes deben ajustarse al proyecto real.

## 1. Implementar usando `Build`

Una vez revisado y aprobado el plan, cambia al agente primario `Build` utilizando `Tab` o el atajo configurado para cambiar de agente.

Después solicita la implementación:

```text
Implementa el plan diseñado.
```

Si el plan está suficientemente detallado, durante `Build` puede utilizarse un modelo de menor capacidad para reducir el consumo de tokens.

La idea es:

```text
Plan  -> mayor razonamiento
Build -> ejecución del plan
```

Esto no significa que `Build` no requiera razonamiento, sino que gran parte de las decisiones importantes deberían haberse resuelto previamente.

## 2. Revisar la implementación

Después de cada implementación analiza los archivos modificados. Revisa:

- Código generado.
- Arquitectura.
- Dependencias instaladas.
- Configuraciones modificadas.
- Manejo de errores.
- Testing.
- Scripts.
- Comportamiento de la aplicación.
- Archivos nuevos.
- Archivos eliminados.
- Cambios no solicitados.

Verifica la compilación con el comando correspondiente al proyecto. En este ejemplo:

```bash
bun run build
```

Los comandos concretos dependerán de cada proyecto. No asumas que una implementación es correcta únicamente porque compila.

## 3. Realizar ajustes posteriores

Si después de revisar la implementación se requieren cambios adicionales, documenta la revisión en un archivo:

```text
revision.md
```

Por ejemplo:

```text
Revisa los cambios solicitados en @revision.md y diseña
un plan para implementarlos.
```

Para cambios importantes repite el flujo:

```text
Plan
  |
  v
Revisar plan
  |
  v
Build
  |
  v
Revisar implementación
  |
  v
Testing
```

Para un procedimiento detallado de implementación sobre una rama, consulta [Desarrollo local con ramas](../git-github/01-flujo-local-con-ramas.md).

---

[← Anterior](02-planificar-y-revisar-el-plan.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente →](04-validar-con-pruebas.md)
