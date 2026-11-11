# WOW Party — Experimento de disponibilidad operativa

**Proyecto:** Brete Tito 2026 / Modo Norte  
**Estado:** Technical validation PASS · Commercial problem validation: PARTIAL PASS  
**Fecha de validación técnica:** 2026-11-09 / 2026-11-10  
**Primera visita comercial:** 2026-11-11  
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

## 9. Estado actual

**TECHNICAL VALIDATION: PASS**  
**PROBLEM VALIDATION: PARTIAL PASS / STRONG SIGNAL**  
**DECISION-MAKER VALIDATION: PENDING**  
**WILLINGNESS TO PAY: PENDING**

## 10. Próximo gate

Conseguir conversación con el dueño/decisor.

Objetivo:
1. confirmar que él reconoce el problema;
2. mostrar la demo si Adriana ya le comentó la idea;
3. identificar la aplicación real usada para reservas;
4. entender qué significa para ellos “integrar con Facebook o Instagram”;
5. determinar si quieren una solución independiente para WOW;
6. obtener permiso para preparar una propuesta concreta;
7. solo entonces definir alcance y precio.

## 11. Regla de alcance

No añadir nuevas funciones, integración con redes sociales, integración con la aplicación de reservas, nueva web de WOW, pagos, CRM, NFC u otros componentes hasta obtener evidencia del decisor que los justifique.
