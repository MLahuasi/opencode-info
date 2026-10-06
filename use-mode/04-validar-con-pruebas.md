# Validar mediante pruebas

El testing debe formar parte del flujo de desarrollo y no ser una tarea opcional posterior.

> **Ejemplo:** la estructura `./tests/`, los comandos de Bun y la rama `feature/testing` son ilustrativos. Utiliza la estrategia de pruebas definida por tu proyecto.

## 1. Preparar las pruebas

Las pruebas pueden desarrollarse en la rama de la funcionalidad. Si se utiliza una rama independiente, debe integrarse o descartarse explícitamente antes de continuar con otra rama para evitar mezclar cambios accidentalmente.

Ejemplo de rama independiente:

```bash
git switch -c feature/testing
```

En `Plan`, solicita:

```text
Desarrolla un plan para implementar testing automático usando Bun.

Requisitos:

- Todos los tests deben almacenarse en ./tests/.
- La estructura de ./tests/ debe replicar, cuando tenga sentido,
  la estructura existente dentro de ./src/.
- Usa las herramientas de testing incluidas con Bun.
- No agregues dependencias de testing si no son necesarias.
- La aplicación no debe construirse ni publicarse si los tests fallan.
- Revisa primero la estructura actual del proyecto antes de proponer cambios.
```

Después de revisar el plan, cambia a `Build` y solicita:

```text
Implementa el plan diseñado.
```

## 2. Ejecutar las pruebas

Los tests pueden ejecutarse mediante:

```bash
bun test
```

Antes de aprobar los cambios comprueba:

- Qué funcionalidades fueron probadas.
- Qué casos de error fueron cubiertos.
- Si existen tests innecesariamente acoplados a la implementación.
- Si los tests pueden ejecutarse de forma independiente.
- Si un fallo realmente provoca que el proceso de CI se detenga.

La compilación no reemplaza las pruebas y las pruebas no reemplazan la revisión manual. Consulta [GitHub Actions personalizados](../git-github/07-github-actions-personalizados.md) para integrar estas validaciones en CI.

---

[← Anterior](03-implementar-revisar-y-ajustar.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente →](05-versionar-y-colaborar.md)
