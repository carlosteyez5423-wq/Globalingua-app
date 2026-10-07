# Globalingua — Instalación de la puerta segura

Con esto, **tu clave de Google queda guardada en secreto en Netlify** y cada estudiante entra
con un **código personal que vence** cuando termina su curso. Ya no tienen que crear su propia
clave de Google, y todos escuchan la voz natural.

## Qué contiene esta carpeta

```
netlify.toml                  ← configuración (no se toca)
netlify/functions/gemini.mjs  ← la puerta segura (guarda tu clave y valida los códigos)
public/index.html             ← Globalingua
public/ielts/index.html       ← Globalingua IELTS
public/admin.html             ← generador de códigos (para ti)
```

## Antes de empezar, necesitas

1. Una cuenta de **GitHub** (gratis): github.com
2. Tu cuenta de **Netlify** (ya la tienes)
3. Una **clave de Google AI Studio** de un proyecto con **facturación activada** y un tope de gasto mensual.

> ¿Por qué GitHub? Según la documentación oficial de Netlify, las funciones se publican con Git,
> con su programa de línea de comandos o con su API; arrastrar la carpeta no está en esa lista.
> GitHub es la opción que no requiere instalar nada.

---

## PARTE A — Subir los archivos a GitHub

1. Entra a **github.com** e inicia sesión.
2. Arriba a la derecha: **+ → New repository**.
3. Nombre: `globalingua-site`. Elige **Private** (privado). Clic en **Create repository**.
4. En la página que aparece, clic en el enlace **uploading an existing file**.
5. Descomprime este ZIP en tu computador. **Abre la carpeta y arrastra todo su contenido**
   (las carpetas `netlify` y `public`, y el archivo `netlify.toml`) a la ventana de GitHub.
6. Espera a que termine de subir y clic en **Commit changes**.
7. ✅ Revisa: en la página del repositorio deben verse `netlify`, `public` y `netlify.toml`.

## PARTE B — Conectar Netlify

8. Entra a **app.netlify.com** → **Add new project → Import an existing project → GitHub**.
9. Autoriza a Netlify y elige el repositorio `globalingua-site`.
10. No cambies nada de la configuración (ya viene en `netlify.toml`). Clic en **Deploy**.

## PARTE C — Las dos variables secretas

11. En tu sitio de Netlify: **Site configuration → Environment variables → Add a variable**.
12. Crea estas dos (marca **"Contains secret values"** en ambas):

| Nombre | Valor |
|---|---|
| `GEMINI_API_KEY` | Tu clave de Google AI Studio (la del proyecto con facturación) |
| `CODE_SECRET` | Una frase larga que inventes, de 30 caracteres o más. Ejemplo: `mi-escuela-de-idiomas-2026-cafe-con-leche-azul` |

> ⚠️ **Guarda `CODE_SECRET` en un lugar seguro** (por ejemplo, tu gestor de contraseñas).
> Si la cambias o la pierdes, todos los códigos ya entregados dejan de funcionar.

13. Ve a **Deploys → Trigger deploy → Deploy site**. (Es necesario para que tome las variables.)

## PARTE D — Pruébalo (3 comprobaciones)

14. Abre `https://TU-SITIO.netlify.app/.netlify/functions/gemini`
    Debe mostrarte un texto que contiene **METHOD_NOT_ALLOWED**. Eso es buena señal: la puerta existe.
15. Abre `https://TU-SITIO.netlify.app/admin.html`, escribe tu `CODE_SECRET`, tu nombre y una fecha
    lejana (por ejemplo, dentro de un año) y presiona **Generar código**.
16. Abre `https://TU-SITIO.netlify.app/` (Globalingua), pega tu código y habla con el tutor.
    Toca un 🔊 y confirma que la voz es natural. Luego prueba `/ielts/`.

## PARTE E — Dar acceso a un estudiante

1. En `admin.html`, escribe su **nombre** y la **fecha en que termina su curso**.
2. Presiona **Generar código** y luego **Copiar mensaje**. Pégalo en WhatsApp: ya incluye los
   dos enlaces y el código.
3. El estudiante pega el código **una sola vez por dispositivo**. Un mismo código sirve para las dos apps.

**Renovar:** genera un código nuevo con la nueva fecha y envíaselo.

**Bloquear a alguien antes de su fecha:** en Netlify, crea la variable `REVOKED_CODES` con el
*nombre* tal como aparece al inicio de su código (por ejemplo `ana`; varios, separados por coma) y
haz **Trigger deploy**.

## PARTE F — Cuidar el gasto

- En **Google AI Studio → pestaña Spend**, activa un **Monthly spend cap** (tope mensual) y usa
  facturación **prepago** con una cantidad pequeña.
- Para ver quién usa la app: en Netlify, **Logs → Functions → gemini**. Cada petición deja una línea
  como `[gemini] ana gemini-3.8-flash-tts -> 200` (solo nombre y modelo; no se guarda lo que conversan).
- La función solo permite los modelos que usan las apps, limita el tamaño de cada respuesta y
  descarta herramientas que cobran aparte (como la búsqueda web).

## Actualizar las apps más adelante

Entra al repositorio en GitHub → **Add file → Upload files** → sube el archivo nuevo (por ejemplo
`public/index.html`) → **Commit changes**. Netlify publica solo. Cada publicación gasta 15 créditos
del plan gratis (300 al mes), así que junta varios cambios antes de subir.

## Notas importantes

- **La dirección del sitio es nueva.** Tu sitio anterior sigue funcionando con el sistema viejo hasta
  que lo elimines. Para quedarte con el nombre `globalingua.netlify.app`, primero cámbiale el nombre
  al sitio viejo (Site configuration → Change site name) y luego pónselo al nuevo.
- **El progreso guardado no se traslada**: el navegador guarda los datos por dirección web, y cambió.
- Cada estudiante conserva su progreso en su propio dispositivo.
- Los textos para convertir en voz pueden medir hasta ~2,600 caracteres (unos 3 minutos de audio).

## Novedad: Zona Niños 🧒

Dentro de Globalingua hay una pestaña **Niños** con 5 niveles (A1 a C1), 12 temas y 6 actividades
(escena con imágenes, tarjetas de palabras, juego de escuchar, ordenar frases, preguntas para hablar y
chat con Lingo, un amigo de IA). No necesitas configurar nada: usa el mismo código de acceso.

- **Costo:** hay más peticiones de voz por sesión que con adultos. Con tu cuenta de Google con
  facturación y el tope mensual activado, es poco dinero. El contenido que genera la IA se guarda en
  el dispositivo para no pedirlo otra vez.
- **Voz rápida:** con tu puerta segura, el audio de las palabras se precarga en segundo plano. Quien use
  una clave gratuita propia (la que empieza con `AIza`) no tiene esa precarga, para no gastar su cuota diaria.
- **Niños pequeños:** recomienda uso acompañado. El contenido lo genera una IA y no lo revisa una persona.
  Lingo tiene reglas de seguridad (no pide datos personales y sugiere hablar con un adulto de confianza
  si el niño dice algo preocupante), pero ninguna IA es perfecta.
- **Privacidad:** lo que el niño escribe en el chat con Lingo se envía a Google para generar la respuesta.
  Conviene mencionarlo en tus términos y política de privacidad, y avisar a los padres.

## Si algo falla

| Lo que ves | Qué significa |
|---|---|
| "Ese código no es válido" | El código tiene un error al copiarlo, o se generó con una frase secreta distinta a `CODE_SECRET`. Genera uno nuevo. |
| "Tu acceso venció" | Pasó la fecha del código. Genera uno nuevo. |
| `SERVER_NOT_CONFIGURED` | Falta `GEMINI_API_KEY` o `CODE_SECRET`, o no hiciste **Trigger deploy** después de crearlas. |
| "Hubo un error con la API" / "No se pudo conectar con ningún modelo" | Ahora la app muestra la **causa real** entre paréntesis (o en letra pequeña). Si dice *Failed to fetch*, no hay conexión con el servidor. Si dice *API key not valid*, la clave pegada no sirve. Si dice *No se encontró el servidor de la escuela*, abriste el archivo guardado en el teléfono o computador: **un código de escuela solo funciona abriendo la app desde el enlace https de tu sitio de Netlify**. Para probar un archivo local, usa una clave propia de Google (la que empieza con `AIza`). |
| "No se encontró el servidor de la escuela" | La app no está abierta desde tu sitio de Netlify, o la función no se publicó. Revisa en Netlify la pestaña **Functions**. |
| Suena la voz robótica de respaldo | El servicio de voz de Google está saturado o llegaste al límite de tu cuenta. Revisa Spend y cuotas en AI Studio. |
| Error de tiempo (504) a los ~10 segundos | Algunos sitios antiguos de Netlify tienen un límite más corto en funciones. Avísame y lo ajustamos. |
