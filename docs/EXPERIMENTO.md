# WOW Party — Experimento de disponibilidad operativa

**Proyecto:** Brete Tito 2026 / Modo Norte  
**Estado:** CERRADO · Technical validation PASS · Commercial opportunity STOP  
**Fecha de validación técnica:** 2026-11-09 / 2026-11-10  
**Primera visita comercial:** 2026-11-11  
**Cierre comercial:** 2026-11-17  
**Naturaleza:** Prototipo demostrativo no oficial. WOW Party no es cliente de Modo Norte en esta etapa.

## 1. Problema observado

Se identificó una fricción plausible para clientes de un centro de entretenimiento con áreas y salones reservables: una persona puede llegar o pagar sin tener claridad suficiente sobre qué espacios estarán disponibles durante su visita.

La evidencia pública que motivó el experimento incluyó:
- una reseña que describía una situación de disponibilidad parcial comunicada después del pago;
- publicaciones de WOW indicando que horarios y disponibilidad pueden variar por fiestas, reservas o eventos privados;
- recomendación pública del propio negocio de revisar redes o escribir antes de visitar.

La hipótesis no es que exista una falla recurrente, sino que una página operativa podría reducir incertidumbre antes de la visita.

## 2. Hipótesis

Si el negocio dispone de una página simple que muestre horario, disponibilidad de espacios, reservas conocidas, próximos eventos y excepciones, entonces un cliente puede entender rápidamente qué puede usar antes de llegar o pagar.

Para que la solución sea viable operativamente, el encargado no debería tener que actualizar manualmente el estado todos los días ni duplicar información que ya existe en otra herramienta.

## 3. MVP construido

Se desarrolló una landing estática que muestra:
- estado abierto/cerrado calculado automáticamente;
- horario habitual;
- disponibilidad del área de juegos;
- disponibilidad del Salón Nova;
- disponibilidad de un segundo salón;
- reservas programadas con fecha, hora de inicio y fin;
- próximos eventos públicos y reservas privadas;
- promoción vigente;
- nota operativa;
- overrides manuales para excepciones.

## 4. Automatización

La lógica usa este orden de prioridad:

1. override manual;
2. reserva programada;
3. horario habitual.

El horario se calcula en la zona horaria `America/Costa_Rica`.

Una reserva cargada una sola vez cambia automáticamente el estado del salón durante su rango horario, vuelve a dejarlo disponible al finalizar y puede mostrarse anticipadamente en la sección de próximos eventos y reservas.

## 5. Arquitectura validada

```
Encargado
   ↓
Pages CMS
   ↓
GitHub
   ↓
GitHub Actions / deploy
   ↓
sitio estático público
```

El sitio no requiere base de datos pública ni backend propio para este MVP.

## 6. Seguridad y alcance

Principios definidos para una eventual versión de producción:
- un repositorio aislado por cliente;
- repositorio privado en producción;
- mínimo privilegio para editores;
- no almacenar contraseñas, tokens, datos de pago ni datos personales en el contenido;
- MFA/passkeys en cuentas administrativas;
- HTTPS;
- historial Git para recuperación;
- dominio y contenido transferibles al cliente;
- separar cualquier futuro sistema de reservas con datos personales de esta landing operativa.

## 7. Pruebas técnicas realizadas

### Gate 1 — edición móvil
**Objetivo:** cambiar un estado desde Pages CMS utilizando un teléfono sin tocar código ni Git.  
**Resultado:** PASS.

### Gate 2 — publicación automática
**Objetivo:** comprobar que un cambio guardado desde Pages CMS genera un commit y despliega automáticamente el sitio.  
**Resultado:** PASS.

### Gate 3 — reserva programada
**Objetivo:** crear desde el celular una reserva ficticia que incluyera la hora actual y comprobar que la landing mostrara automáticamente el salón como reservado.  
**Resultado:** PASS.

### Gate 4 — próximos eventos y reservas
**Objetivo:** mostrar anticipadamente reservas privadas y eventos públicos futuros sin revelar datos personales.  
**Resultado:** IMPLEMENTADO; pendiente de validación comercial de utilidad con decisor.

## 8. Primera visita comercial — 2026-11-11

Mario visitó WOW Party y conversó con Adriana, esposa del dueño y persona a cargo del local en ese momento. El esposo es quien toma las decisiones.

### Evidencia obtenida

- Adriana consideró la demo una idea útil para facilitar el trabajo del negocio y permitir a clientes confirmar disponibilidad.
- Indicó que algunos clientes se molestan cuando llegan y descubren que no existe disponibilidad completa.
- Al preguntar si era algo que les interesaría desarrollar, indicó que debe discutirlo con su esposo.
- Solicitó la información de contacto de Mario.
- Indicó que Mario puede escribir al número de WOW.
- Mario dejó abierta la posibilidad de reunirse con el dueño y mostrarle la demo.

### Operación actual descubierta

- Las reservas no se manejan mediante el Google Calendar público de Ameniza tu Fiesta.
- Las reservas se gestionan mediante una aplicación, cuyo nombre Adriana no conocía.
- Hay una persona encargada del manejo de reservas.
- Hay una persona encargada de redes sociales.
- El sitio Ameniza tu Fiesta es administrado por el esposo.
- WOW y Cajita Mágica pertenecen a los mismos dueños, pero existe interés en trabajarlos de forma más separada.
- Adriana preguntó si la solución podía integrarse con Facebook o Instagram.

### Interpretación

**Problema operativo:** VALIDADO cualitativamente.  
Existe al menos una persona del negocio que reconoce la utilidad del problema/solución y confirma fricción real con clientes por disponibilidad.

**Interés comercial:** PARCIALMENTE VALIDADO.  
Hay apertura, solicitud de contacto y autorización implícita para seguimiento, pero no existe todavía decisión del decisor ni disposición a pagar confirmada.

**Disposición a pagar:** NO VALIDADA.

**Integración técnica:** NO VALIDADA.  
No debe construirse integración con redes sociales ni con la aplicación de reservas antes de conocer la herramienta real y hablar con el decisor.

## 9. Segunda visita y cierre comercial — 2026-11-17

Mario conversó con el esposo de Adriana, dueño y decisor de WOW Party.

### Evidencia obtenida

- El decisor es ingeniero en sistemas.
- WOW ya tiene un proyecto en marcha para desarrollar una página web propia.
- Ese proyecto contempla precisamente la integración operativa para informar disponibilidad de la sala.
- La conversación terminó con un rechazo comercial cortés; no existe oportunidad actual para Modo Norte.

### Interpretación

**Problema operativo:** VALIDADO.  
La necesidad identificada coincide con una solución que el propio negocio ya decidió construir.

**Solución conceptual:** VALIDADA INDIRECTAMENTE.  
La dirección del proyecto interno de WOW coincide con la hipótesis de Modo Norte: web propia + disponibilidad operativa.

**Oportunidad comercial para Modo Norte:** STOP.  
El negocio tiene capacidad técnica interna y una solución ya en desarrollo.

**Disposición a pagar a Modo Norte:** NO VALIDADA / oportunidad cerrada.

## 10. Estado final

**TECHNICAL VALIDATION: PASS**  
**PROBLEM VALIDATION: PASS**  
**SOLUTION DIRECTION: VALIDATED INDIRECTLY**  
**COMMERCIAL OPPORTUNITY: STOP**  
**REASON:** solución interna ya en desarrollo.

## 11. Aprendizajes

1. Validar el problema antes de construir.
2. Llegar al decisor antes de invertir desarrollo significativo.
3. Un usuario/interesado puede confirmar utilidad sin tener poder de compra.
4. El cliente ideal necesita no solo el problema, sino también ausencia de una solución interna suficiente.
5. Antes de proponer una solución, investigar proceso actual, herramientas, responsable y proyectos en curso.
6. No convertir una demo útil en una excusa para buscar negocios que encajen artificialmente con ella.

## 12. Regla para próximos experimentos

Secuencia obligatoria:

```
fricción observable → conversación de descubrimiento → problema confirmado
→ decisor → solución propuesta → precio → construcción
```

No construir nuevas funciones ni demos personalizadas antes de confirmar problema y decisor.

La demo WOW queda congelada como pieza demostrativa de capacidad técnica y aprendizaje del experimento.
