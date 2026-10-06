# [Aprobación semiautomática de Specs](https://opencode.ai/docs/github/#_top)

Funciona cuando se sube una Spec sin ejecutar previamente `@spec-acceptance-validator`.

1. Instalar OpenCode:

```bash
opencode github install
```

2. Se abre url de configuración:

![](../assets/22-opencode-revision-spec-semi-automatic-configure.png)

3. Seleccionar en donde se quiere instalar (ambiente)

4. Aprobar ingreso a `GitHub`

5. Añadir el repositorio en el que se quiere automatizar la aprobación de `Specs` de forma semiautomática.

![](../assets/23-opencode-revision-spec-semi-automatic-add-repo.png)

6. Seleccionar proveedor.

![](../assets/24-opencode-revision-spec-semi-automatic-choise-llm.png)

7. Seleccionar modelo.

![](../assets/25-opencode-revision-spec-semi-automatic-choise-model.png)

8. Seguir los pasos indicados por la configuración.

![](../assets/26-opencode-revision-spec-semi-automatic-next-steps.png)

9. Crear un `new repository secret`.

![](../assets/27-opencode-revision-spec-semi-automatic-github-secret.png)

10. Pegar la API key del proveedor para crear `OPENAI_API_KEY`.

![](../assets/28-opencode-revision-spec-semi-automatic-github-openai-api-key.png)

11. Crear una nueva rama, hacer commit y subir la rama para realizar un `pull request`

```bash
git checkout -b 01-actions
git add .
git commit -m "OpenDay - Aprobación semiautomática de specs"
git push -u origin 01-actions
```

12. Aprobar `spec` en `GitHub`

![](../assets/29-opencode-revision-spec-semi-automatic-github-pull-request.png)

13. Actualizar cambios en `main` local, eliminar la rama temporal y eliminar la rama remota:

```bash
git switch main
git pull
git branch -d 01-actions
git push origin --delete 01-actions
```

El comando `git branch -d origin 01-actions` conserva el ejemplo original, pero contiene un error: `git branch -d` debe recibir el nombre de la rama local. La forma corregida es:

```bash
git branch -d 01-actions
```

14. Una vez configurado el `workflow de opencode`, se debe crear un `issue`

![](../assets/30-opencode-revision-spec-semi-automatic-github-add-issue.png)

15. Se añade un comentario anteponiendo el comando `/oc` u `/opencode` y especificando el agente [`spec-acceptance-validator`](https://github.com/MLahuasi/opencode-daycare/blob/main/.opencode/agent/spec-acceptance-validator.md) que se quiere ejecutar:

![](../assets/31-opencode-revision-spec-semi-automatic-github-add-issue-comment.png)

---

[← Anterior](../08-opencode-github.md) | [Temario](../08-opencode-github.md) | [Siguiente →](../09-opencode-spec-driven-development.md)
