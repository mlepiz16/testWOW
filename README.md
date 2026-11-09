# WOW Party — Pages CMS microprueba

## Objetivo
Validar si un encargado puede cambiar el estado operativo desde el celular en menos de 30 segundos, sin tocar código ni Git.

## Arquitectura
Pages CMS → GitHub → Netlify/Cloudflare Pages → landing pública.

## Archivos
- `.pages.yml`: campos editables y dropdowns.
- `data/status.json`: única fuente de datos operativos.
- `index.html`: landing pública.

## Prueba
1. Conectar este repositorio a Netlify o Cloudflare Pages.
2. Abrir https://app.pagescms.org e iniciar sesión con GitHub.
3. Dar acceso a este repositorio.
4. Abrir **Estado operativo**.
5. Desde el celular cambiar, por ejemplo, **Salón Nova → Disponible**.
6. Guardar.
7. Esperar el redeploy y recargar la landing.

## KEEP
- edición móvil menor a 30 s;
- no tocar Git ni código;
- cambio visible en la landing;
- campos principales sin texto libre;
- flujo comprensible para una persona no técnica.

## DROP
- login/edición engorrosos;
- redeploy demasiado lento o poco confiable;
- requiere entender Git/repositorios;
- soporte recurrente desproporcionado.
