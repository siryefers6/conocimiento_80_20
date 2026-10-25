```html
{{ variable }}
```

Ejemplo:

```python
return templates.TemplateResponse(
    request=request,
    name="index.html",
    context={"nombre": "Abathur"}
)
```

```html
<p>{{ nombre }}</p>
```

Resultado: `Abathur`