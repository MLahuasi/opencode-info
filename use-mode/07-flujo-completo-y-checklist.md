# Flujo completo y checklist

> **Ejemplo:** el flujo y el checklist resumen una práctica recomendada, pero no sustituyen las reglas, revisiones y criterios específicos de cada proyecto.

## 1. Flujo completo recomendado

El proceso completo puede resumirse así:

```text
README.md / documentación
          |
          v
Configuración inicial manual
          |
          v
       /init
          |
          v
      AGENTS.md
          |
          v
     Revisar reglas
          |
          v
       Crear rama
          |
          v
         Plan
          |
          v
     Revisar plan
          |
          v
         Build
          |
          v
    Revisar código
          |
          v
       Testing
          |
          v
        Commit
          |
          v
         Push
          |
          v
   Pull Request / CI
          |
          v
        Merge
          |
          v
   GitHub Actions
          |
          v
       Release
```

## 2. Checklist de revisión

Antes de aprobar una implementación generada por OpenCode comprueba:

### Código

- [ ] ¿Se entiende el código generado?
- [ ] ¿Se comprende qué hace cada componente importante?
- [ ] ¿Se conocen las limitaciones de la implementación?
- [ ] ¿Se agregaron funcionalidades que no fueron solicitadas?
- [ ] ¿La arquitectura sigue siendo coherente?
- [ ] ¿El código puede simplificarse?

### Dependencias

- [ ] ¿OpenCode instaló nuevas dependencias?
- [ ] ¿Qué dependencias instaló?
- [ ] ¿Son realmente necesarias?
- [ ] ¿Existe una solución utilizando herramientas ya disponibles?

### Configuración

- [ ] ¿Se modificaron archivos de configuración?
- [ ] ¿Los cambios eran necesarios?
- [ ] ¿Se agregaron variables de entorno?
- [ ] ¿Se evitaron secretos dentro del repositorio?

### Testing

- [ ] ¿Se ejecutaron los tests?
- [ ] ¿Qué comportamiento cubren?
- [ ] ¿Se probaron casos de error?
- [ ] ¿La nueva funcionalidad rompió otra existente?
- [ ] ¿El CI falla correctamente cuando un test falla?

### Build

- [ ] ¿La aplicación compila correctamente?
- [ ] ¿La aplicación se ejecutó manualmente?
- [ ] ¿Los ejecutables generados funcionan?
- [ ] ¿Se probaron las plataformas soportadas?

### Git

- [ ] ¿Se creó una rama antes de realizar cambios importantes?
- [ ] ¿Los cambios fueron revisados antes del merge?
- [ ] ¿Se revisó `git diff`?
- [ ] ¿Se revisó `git diff --cached` antes del commit?
- [ ] ¿El commit representa correctamente los cambios?
- [ ] ¿Sería conveniente utilizar un Pull Request?

### GitHub Actions y releases

- [ ] ¿El workflow ejecuta los tests antes de publicar?
- [ ] ¿El workflow se detiene ante errores?
- [ ] ¿Los permisos utilizados son los mínimos necesarios?
- [ ] ¿Los releases contienen únicamente los archivos esperados?
- [ ] ¿No existen secretos dentro de los artefactos?
- [ ] ¿La procedencia e integridad de los artefactos fueron revisadas?

---

> **Regla principal:** OpenCode puede acelerar considerablemente el desarrollo, pero la responsabilidad sobre las decisiones, el código, las dependencias, las pruebas, la seguridad y los artefactos publicados continúa siendo del desarrollador.

---

[← Anterior](06-automatizar-compilaciones-y-publicaciones.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente capítulo →](../08-opencode-github.md)
