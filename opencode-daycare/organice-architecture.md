### Reorganizar Arquitectura

- En modo `Plan`

```
/spec

Se debe reorganizar la aplicación porque la estructura actual es confusa. Agrupa y separa claramente páginas, componentes, `shared` y otras capas como `infrastructure`.
Revisa también la organización de los `types`. Por ejemplo, `@features/kids/types/kids.ts` contiene `Person`, `Kid` y `Parent`; aunque están relacionados, representan conceptos distintos y deberían separarse.
Diseña un plan para implementar esta reorganización de forma incremental, simple y mantenible.
```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [08-application-architecture.md](../../open-daycare/specs/08-application-architecture.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/
```

- Cuando crea la rama `spec-08-application-architecture` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-08-application-architecture"
```

- También se creó el plan:

```md
### PLAN

1. Registrar línea base de rutas, JSON, imports y smoke tests.
2. Corregir ciclo de Auth.
3. Retirar reexports de fixtures desde Feed y Layout.
4. Definir entradas client-safe y server.
5. Separar tipos de Kids.
6. Crear People.
7. Crear Rooms.
8. Crear Family.
9. Reubicar contratos de formulario.
10. Auditar tipos Auth y Feed.
11. Mover StaffSidebar a components/layout.
12. Mover navegación a shared/config.
13. Pasar fixtures de Feed desde Home.
14. Eliminar Layout antiguo.
15. Extraer adapter JSON.
16. Adaptar servicios de dominio.
17. Mover JSON a Infrastructure.
18. Verificar flujos de Kids y activación.
19. Crear /home y redirect raíz.
20. Mover Auth y redirects.
21. Mover Kids bajo (staff).
22. Mover Edit y redirect.
23. Eliminar archivos obsoletos.
24. Actualizar documentación.
25. Revisar arquitectura y límites.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-08-application-architecture - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-08-application-architecture
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-08-application-architecture
```
