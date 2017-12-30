---
title: Responsive Email
layout: post
---

Pre-procesador de Gmail añade sus propios estilos al correo.
Gmail no da soporte a estilos externos o la etiqueta <style>, por lo tanto es necesario usar estilos en linea.
Se usarán tablas debido a que muchos.

Se usara la version simplificada para hacer tablas. De igual manera muchos clientes 

```html
<table border ="0" cellpadding="0" cellspacing="0">
	<tr>
		<td></td>
	</tr>
</table>
```

Debido a que soporte de rendero y el mínimo soporte de estilos css en etiquetas html a excepticon de tables y celdas de tabla, se usará para el diseño de los correos esta última.