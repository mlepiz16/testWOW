# WOW Party — Experimento de disponibilidad operativa

**Proyecto:** Brete Tito 2026 / Modo Norte  
**Estado:** Technical validation PASS  
**Fecha de validación:** 2026-11-09 / 2026-11-10  
**Naturaleza:** Prototipo demostrativo no oficial. WOW Party no es cliente de Modo Norte en esta etapa.

## 1. Problema observado

Se identificó una fricción plausible para clientes de un centro de entretenimiento con áreas y salones reservables: una persona puede llegar o pagar sin tener claridad suficiente sobre qué espacios estarán disponibles durante su visita.

La evidencia que motivó el experimento incluyó una reseña pública que describía una situación de disponibilidad parcial comunicada después del pago.

La hipótesis no es que exista una falla recurrente, sino que una página operativa podría reducir incertidumbre antes de la visita.

## 2. Hipótesis

Si el negocio dispone de una página simple que muestre horario, disponibilidad de espacios, reservas conocidas y excepciones, entonces un cliente puede entender rápidamente qué puede usar antes de llegar o pagar.

Para que la solución sea viable operativamente, el encargado no debería tener que actualizar manualmente el estado todos los días.

## 3. MVP construido

Se desarrolló una landing estática que muestra:

- estado abierto/cerrado calculado automáticamente;
- horario habitual;
- disponibilidad del área de juegos;
- disponibilidad del Salón Nova;
- disponibilidad de un segundo salón;
- reservas programadas con fecha, hora de inicio y fin;
- promoción vigente;
- nota operativa;
- overrides manuales para excepciones.

## 4. Automatización

La lógica usa este orden de prioridad:

1. override manual;
2. reserva programada;
3. horario habitual.

El horario se calcula en la zona horaria `America/Costa_Rica`.

Una reserva cargada una sola vez cambia automáticamente el estado del salón durante su rango horario y vuelve a dejarlo disponible al finalizar.

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

## 7. Pruebas realizadas

### Gate 1 — edición móvil

**Objetivo:** cambiar un estado desde Pages CMS utilizando un teléfono sin tocar código ni Git.

**Resultado:** PASS.

### Gate 2 — publicación automática

**Objetivo:** comprobar que un cambio guardado desde Pages CMS genera un commit y despliega automáticamente el sitio.

**Resultado:** PASS.

### Gate 3 — reserva programada

**Objetivo:** crear desde el celular una reserva ficticia que incluyera la hora actual y comprobar que la landing mostrara automáticamente el salón como reservado.

**Resultado:** PASS.

## 8. Resultado actual

**TECHNICAL VALIDATION: PASS**

Se validó el circuito:

```
móvil → Pages CMS → GitHub → deploy → sitio actualizado
```

y también:

```
reserva programada → cálculo automático → disponibilidad correcta
```

## 9. Lo que todavía NO está validado

- que WOW Party perciba el problema como suficientemente importante;
- que el negocio quiera adoptar la solución;
- disposición a pagar;
- frecuencia real de actualización;
- sistema que utilizan actualmente para administrar reservas;
- integración con Google Calendar u otra fuente;
- arquitectura definitiva de producción.

## 10. Próximo gate

Validación comercial con negocio real.

Antes de agregar nuevas funciones:

1. preparar una demo visual clara;
2. explicar el problema y el beneficio en lenguaje no técnico;
3. preguntar cómo administran actualmente las reservaciones;
4. comprobar si la solución reduce trabajo o incertidumbre;
5. medir interés real antes de invertir más tiempo técnico.

## 11. Regla de alcance

No añadir reservas completas, pagos, CRM, automatizaciones externas, NFC, reseñas u otras funciones hasta obtener evidencia comercial que las justifique.
