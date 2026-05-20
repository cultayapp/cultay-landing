# Despliegue de la landing de cultay. en Vercel

Dos caminos. El **Camino A** (GitHub + Vercel) es el recomendado si quieres despliegues automáticos cada vez que edites algo. El **Camino B** (Vercel CLI) es el más rápido si solo quieres publicar ahora.

---

## Camino A — GitHub + Vercel (recomendado)

### 1. Crea el repositorio en GitHub

1. Ve a https://github.com/new
2. Nombre del repositorio: `cultay-landing` (o el que prefieras)
3. Visibilidad: **Private**
4. No marques ningún checkbox de inicialización (README, .gitignore, etc.)
5. Haz clic en **Create repository**

### 2. Sube la carpeta `landing/` al repositorio

Abre Terminal y ejecuta:

```bash
# Navega a la carpeta landing
cd "/Users/martaa/Documents/Claude/Projects/APP pelis y libros/landing"

# Inicializa git
git init
git add .
git commit -m "feat: landing page inicial cultay."

# Conecta con GitHub (reemplaza TU_USUARIO con tu usuario de GitHub)
git remote add origin https://github.com/TU_USUARIO/cultay-landing.git
git branch -M main
git push -u origin main
```

### 3. Despliega en Vercel

1. Ve a https://vercel.com y entra con tu cuenta de GitHub
2. Haz clic en **Add New → Project**
3. Busca y selecciona el repo `cultay-landing`
4. Configuración del proyecto:
   - **Framework Preset**: Other (es HTML estático, no hay framework)
   - **Root Directory**: `.` (la raíz, que ya es la carpeta landing)
   - **Build Command**: *(déjalo vacío)*
   - **Output Directory**: `.`
5. Haz clic en **Deploy**

En ~30 segundos tendrás la URL: `cultay-landing.vercel.app`

### 4. Añade tu dominio personalizado (opcional)

Si tienes el dominio `cultay.app` (o similar):

1. En el dashboard de Vercel → tu proyecto → **Settings → Domains**
2. Escribe `cultay.app` y haz clic en **Add**
3. Vercel te dará dos registros DNS:
   - Un registro **A** apuntando a `76.76.21.21`
   - Un registro **CNAME** para `www` apuntando a `cname.vercel-dns.com`
4. Añade esos registros en el panel de tu registrador de dominio (Namecheap, Cloudflare, etc.)
5. La propagación tarda entre 5 minutos y 24 horas

---

## Camino B — Vercel CLI (más rápido, sin GitHub)

### 1. Instala Vercel CLI

```bash
npm install -g vercel
```

### 2. Despliega

```bash
cd "/Users/martaa/Documents/Claude/Projects/APP pelis y libros/landing"
vercel
```

La primera vez te pedirá que inicies sesión con tu cuenta de Vercel (abre el navegador automáticamente).

Responde así a las preguntas interactivas:

```
? Set up and deploy? → Y
? Which scope? → (elige tu cuenta personal)
? Link to existing project? → N
? What's your project's name? → cultay-landing
? In which directory is your code located? → ./
? Want to modify settings? → N
```

Para el **dominio definitivo** (sin el sufijo `-git` de preview):
```bash
vercel --prod
```

---

## Conectar el formulario de waitlist

El formulario ahora mismo solo hace `console.log`. Para capturar emails reales, hay tres opciones:

### Opción 1 — Formspark (más simple, gratis hasta 250 envíos/mes)

1. Ve a https://formspark.io y crea una cuenta
2. Crea un nuevo formulario → copia el **Form ID**
3. En `index.html`, cambia la función `handleWaitlistSubmit`:

```javascript
async function handleWaitlistSubmit(e) {
  e.preventDefault();
  const email = e.target.querySelector('input[type="email"]').value;
  
  await fetch('https://submit-form.com/TU_FORM_ID', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
    body: JSON.stringify({ email })
  });
  
  e.target.style.display = 'none';
  document.getElementById('success-msg').classList.add('show');
}
```

Haz lo mismo con `handleHeroSubmit`. Sustituye `TU_FORM_ID` por el ID real.

### Opción 2 — Mailchimp (si quieres newsletter)

1. Crea una audiencia en Mailchimp
2. Ve a **Audience → Signup forms → Embedded forms**
3. Copia el `action` URL del form (algo como `https://xxxx.us1.list-manage.com/subscribe/post?u=...&id=...`)
4. En el HTML, cambia el `action` de los dos formularios a esa URL y deja el método `POST` sin JavaScript especial

### Opción 3 — Supabase (si ya lo usas para la app)

Si el stack del MVP usa Supabase, lo más coherente es guardar los emails en una tabla `waitlist`:

```javascript
// Necesitas incluir el SDK de Supabase antes de este script
const supabase = supabase.createClient('TU_URL', 'TU_ANON_KEY');

async function handleWaitlistSubmit(e) {
  e.preventDefault();
  const email = e.target.querySelector('input[type="email"]').value;
  await supabase.from('waitlist').insert({ email });
  e.target.style.display = 'none';
  document.getElementById('success-msg').classList.add('show');
}
```

---

## Actualizar la landing después del despliegue

Si usaste el **Camino A** (GitHub):
- Edita `index.html` en tu ordenador → `git add . && git commit -m "update" && git push`
- Vercel detecta el push y redespliegue automáticamente en ~30 segundos

Si usaste el **Camino B** (CLI):
- Edita el archivo → vuelve a ejecutar `vercel --prod`

---

## Checklist antes de hacer público el enlace

- [ ] Formulario conectado (Formspark / Mailchimp / Supabase)
- [ ] Enlace de Instagram actualizado en el footer
- [ ] Email `hola@cultay.app` en el footer activo (o cambiado por el tuyo)
- [ ] Dominio personalizado configurado (si tienes uno)
- [ ] Abre la página en el móvil y verifica que se ve bien
- [ ] Prueba el formulario de waitlist y comprueba que llega el email
