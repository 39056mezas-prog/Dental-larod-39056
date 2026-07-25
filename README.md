# Dental LAROD — Backend de reservas

Mismo patrón que ya conoces si armaste el de Clipper Barber Studio: el sitio
llama a `/api/availability` para mostrar horarios ocupados, y a `/api/book`
al confirmar una cita. Sin cobro — la cita se crea directo en el calendario
del consultorio.

## Lo que hace falta (nada de esto lo puedo generar yo)
1. **Un proyecto de Google Cloud** con la API de Calendar activada y una
   cuenta de servicio — igual que hicieron para Clipper. Si ya tienen el
   proceso fresco, son los mismos pasos, en un proyecto nuevo.
2. **Compartir el calendario real de LAROD** con el correo de esa cuenta de
   servicio, permiso "Hacer cambios en eventos".
3. Pegar `GOOGLE_SERVICE_ACCOUNT_JSON` y `GOOGLE_CALENDAR_ID` en Vercel →
   Settings → Environment Variables (o en `.env` para probar en local).

## Ojo con el horario
En `lib/time.js` puse Lun–Vie 9:00 AM–6:00 PM y Sáb 9:00 AM–2:00 PM, cerrado
domingo — es mi mejor estimado para un consultorio dental típico, pero **no
me lo confirmaron**, así que revísalo antes de publicar. Si algún servicio
(como una endodoncia) necesita más tiempo que otro, dime y separo la
duración por tipo de consulta en vez de los 45 minutos parejos que tiene
ahorita (`DEFAULT_DURATION_MINUTES`).

## Desplegar
Mismo patrón que Clipper: esta carpeta (`api/` + `lib/` + `package.json`)
va en el mismo repositorio que `index.html`, se conecta ese repo en
vercel.com, y se pegan las variables de entorno.

```
tu-repo/
  index.html
  assets/
    larod-logo.jpg
    larod-exterior.jpg
    larod-team.jpg
    larod-patient-care.jpg
    larod-intraoral-clinical.jpg
  api/
    availability.js
    book.js
  lib/
    calendars.js
    googleCalendar.js
    time.js
  package.json
```

## Pendientes conocidos
- **Un calendario por dentista**: por ahora todo el consultorio comparte un
  calendario (`GOOGLE_CALENDAR_ID`). Si en algún momento cada dentista de la
  familia quiere su propia agenda, `lib/calendars.js` ya está listo para
  extenderse a una lista — solo hace falta agregar los calendarios.
- **Confirmación al paciente**: si captura su correo en el paso 6, se le
  agrega como invitado al evento y Google le manda la confirmación solo —
  no hace falta un servicio de email aparte.
- **Zona horaria**: ya resuelta con `America/Tijuana` explícito (usa
  `luxon`, no matemática de horario de verano a mano).
