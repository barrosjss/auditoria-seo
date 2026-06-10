# Auditoría SEO + GEO — Packway (Pre-Sales Suite)

**Fecha:** 10 de junio de 2026  
**URL auditada:** https://packway.us/  
**Tipo:** Auditoría inicial (técnica, on-page, contenido, GEO)  
**Auditor:** Documentación interna

---

## Resumen ejecutivo

| Área | Puntuación estimada | Estado |
|------|---------------------|--------|
| Infraestructura / dominio | 🔴 Crítico | www redirige a Azure, no a packway.us |
| Indexación | 🔴 Crítico | Sin presencia en `site:packway.us` |
| SEO técnico | 🔴 Crítico | Sin robots, sitemap, canonical, OG, schema |
| On-page | 🟡 Medio | Buen contenido, title largo, placeholders |
| Contenido / marca | 🟡 Medio | Copy sólido, secciones incompletas |
| GEO (LLMs) | 🔴 Crítico | Sin llms.txt ni entidad estructurada |

**Conclusión:** Packway tiene una landing bien escrita para su nicho (pre-sales automation en packaging), pero hoy **no funciona como activo SEO**. El dominio `www` envía tráfico y bots a una URL de Azure; no hay archivos técnicos básicos; Google parece indexar el subdominio de Azure y no `packway.us`. Antes de invertir en contenido o link building, hay que consolidar dominio, canonical y señales técnicas.

---

## 1. Hallazgos críticos (acción inmediata)

### 1.1 🔴 Redirect www → URL de Azure (no al dominio de marca)

| URL | Comportamiento |
|-----|----------------|
| `https://packway.us` | ✅ 200 — sirve la landing correctamente |
| `https://www.packway.us` | ❌ 301 → `https://app-pss-landing-page-c7b2eye9b7e2ajbt.canadacentral-01.azurewebsites.net/` |

**Impacto:**
- Usuarios que escriben `www` acaban en un subdominio de Azure, no en `packway.us`
- Google puede indexar la URL de Azure como URL canónica (ya aparece en resultados de búsqueda)
- Dilución de autoridad de dominio entre packway.us y azurewebsites.net
- Mala señal de marca y confianza B2B

**Acción:** Configurar DNS/hosting para que `www.packway.us` → 301 → `https://packway.us/` (nunca a Azure). El subdominio Azure debe ser solo backend, no URL pública indexable.

---

### 1.2 🔴 Sin indexación en el dominio de marca

- `site:packway.us` → **0 resultados**
- Búsqueda genérica "Packway Pre-Sales Suite" → Google muestra la URL de **Azure**, no packway.us

**Acción:** Verificar Google Search Console para `packway.us`, corregir redirects, añadir sitemap, solicitar indexación de la homepage.

---

### 1.3 🔴 robots.txt, sitemap.xml y llms.txt inexistentes

Todas estas rutas devuelven el **mismo index.html** (SPA fallback):

| Ruta | Content-Type | Content-Length |
|------|--------------|----------------|
| `/robots.txt` | text/html | 119.554 bytes |
| `/sitemap.xml` | text/html | 119.554 bytes |
| `/llms.txt` | text/html | 119.554 bytes |
| `/favicon.ico` | text/html | 119.554 bytes |
| `/privacy` | text/html | 119.554 bytes |

**Impacto:**
- Crawlers no reciben instrucciones de indexación
- No hay mapa de URLs para Google
- LLMs no tienen archivo de descubrimiento (GEO)
- Favicon roto en navegadores

**Acción:** Servir archivos estáticos reales en Azure/Next.js (`public/robots.txt`, `public/sitemap.xml`, `public/llms.txt`, `public/favicon.ico`).

---

### 1.4 🔴 Sin canonical, Open Graph, Twitter Cards ni Schema.org

Elementos ausentes en el `<head>`:

- `rel="canonical"`
- `meta property="og:*"`
- `meta name="twitter:*"`
- `application/ld+json` (Organization, SoftwareApplication, WebSite)

**Impacto:** Al compartir en LinkedIn/email no hay preview controlado. Los motores generativos no tienen datos estructurados para citar el producto correctamente.

---

## 2. SEO técnico

### 2.1 Infraestructura

| Check | Resultado |
|-------|-----------|
| HTTPS | ✅ Activo en packway.us |
| Stack | Next.js static export (`/_next/static/chunks/...`) |
| Hosting | Azure App Service (`canadacentral-01.azurewebsites.net`) |
| Generador | `v0.app` (meta generator) |
| Assets | Vercel Blob Storage (`hebbkx1anhila5yf.public.blob.vercel-storage.com`) |
| Videos | Thumbnails Vimeo CDN |
| Tipo de sitio | Single Page Application (1 URL, navegación por anchors) |
| robots.txt | ❌ No existe (fallback HTML) |
| sitemap.xml | ❌ No existe |
| llms.txt | ❌ No existe |
| Canonical | ❌ Ausente |
| hreflang | ❌ Solo `lang="en"` — sin versiones alternativas |

### 2.2 Meta tags homepage

| Tag | Valor | Evaluación |
|-----|-------|------------|
| Title | Packway Pre-Sales Suite \| Software for Packaging Sales & Design Teams | 🟡 69 chars — ligeramente largo (ideal ≤60) |
| Meta description | Specialized software for paper-based packaging sales, pre-sales & design teams. For corrugated, folding carton, and display manufacturers. | ✅ 138 chars — OK |
| robots | (no declarado) | 🟡 Asume index,follow por defecto |
| viewport | width=device-width, initial-scale=1 | ✅ OK |

### 2.3 Estructura de URLs

**Única URL indexable:** `https://packway.us/`

Navegación interna por anclas:

| Anchor | Sección |
|--------|---------|
| `#modules` | Packway Estimating, Docupoint, Integration Layer |
| `#solutions` | Videos demo |
| `#what-is-presales` | Reto pre-sales |
| `#partners` | BBI + Aurum |
| `#contact` | Formulario |
| `#customers` | Logos clientes |

**Enlaces externos relevantes:**
- `https://bbinternational.com/en/digital-formula/packway-eng/estimating/`
- `https://bbinternational.com/en/digital-formula/packway-eng/crm-and-technical-team/`
- `https://bbinternational.com`
- `https://aurumintelligence.com`

**Problema:** Todo el contenido profundo de producto vive en BBI International, no en packway.us. Packway.us es solo landing; no compite por keywords de producto sin páginas propias.

### 2.4 Jerarquía de headings

| Nivel | Cantidad | Evaluación |
|-------|----------|------------|
| H1 | 1 — "Pre-Sales Automation for Packaging Converters" | ✅ Correcto |
| H2 | 5 | ✅ Estructura lógica |
| H3 | 21 | ✅ Detalle por módulo/beneficio |

Jerarquía semántica coherente, sin saltos graves.

### 2.5 Imágenes

- ✅ Alt text presente en imágenes revisadas (logo, hero, videos, partners)
- Assets en CDN externo (Vercel Blob) — dependencia de terceros
- Thumbnails de Vimeo para sección de demos

### 2.6 Seguridad

- ✅ No se detectó malware ni scripts sospechosos (a diferencia de otros sitios del ecosistema)
- Scripts solo de `/_next/static/chunks/` (Next.js propio)
- Formulario de contacto sin `action` visible en HTML — probablemente manejado por JS; verificar que envíe datos de forma segura (HTTPS, sin exponer keys)

---

## 3. SEO on-page y contenido

### 3.1 Fortalezas

- **Propuesta de valor clara:** pre-sales automation para corrugated, displays, folding carton
- **Módulos bien definidos:** Packway Estimating, Docupoint, Integration Layer
- **Keywords de nicho** integradas de forma natural: ECMA, FEFCO, ArtiosCAD, RFQ, win-rate
- **Prueba social:** 30+ años, 220+ converters, 30 profesionales
- **Videos demo** en Vimeo (4 activos)
- **Partners creíbles:** BBI International (desarrollador) + Aurum Intelligence
- **CTA visible:** "Get Free Consultation", formulario de contacto

### 3.2 Debilidades

| Problema | Detalle |
|----------|---------|
| Placeholders visibles | "Solution Title 5/6" marcados **Coming soon** |
| Copy genérico en contacto | "Let's talk about measurable transformation" — idéntico a Aurum Intelligence |
| Teléfono compartido | 1-562-784-4379 — mismo número que Aurum (puede confundir entidades) |
| Enlace partner incorrecto | Apunta a `aurumintelligence.com` (dominio distinto de `.net`) |
| Mensaje geográfico mixto | Dominio `.us` pero stat "220+ Converters in **Europe**" |
| Sin página legal | No hay Privacy Policy / Terms (ruta `/privacy` devuelve homepage) |
| Sin blog / recursos | Cero contenido evergreen para captar tráfico informacional |
| Contenido duplicado potencial | Mucho overlap con páginas de BBI sobre Packway |
| Footer link roto | `#features` — sección no verificada en nav principal |

### 3.3 Keywords / oportunidades

Términos objetivo donde packway.us debería existir con páginas dedicadas:

- packaging pre-sales software
- corrugated estimating software
- packaging RFQ automation
- Docupoint CRM packaging
- ArtiosCAD quoting integration
- folding carton pre-sales automation

**Recomendación:** Crear al menos 3 landing pages independientes (`/estimating/`, `/docupoint/`, `/integration/`) con schema `SoftwareApplication` cada una.

---

## 4. Auditoría GEO (Generative Engine Optimization)

GEO = optimización para que LLMs (ChatGPT, Perplexity, Gemini, Claude, Copilot) entiendan, citen y recomienden correctamente la marca/producto.

### 4.1 Estado actual

| Señal GEO | Estado |
|-----------|--------|
| llms.txt | ❌ No existe (fallback HTML) |
| llms-full.txt | ❌ No existe |
| Schema Organization | ❌ |
| Schema SoftwareApplication | ❌ |
| Schema FAQPage | ❌ (hay contenido FAQ-like en "Pre-Sales Challenges") |
| Entidad clara en HTML | 🟡 Parcial — texto descriptivo bueno, sin metadatos |
| URL canónica estable | ❌ Conflicto packway.us vs Azure |
| Contenido citables | ✅ Stats y definiciones de módulos |
| Autoridad externa | 🟡 BBI tiene contenido indexado; packway.us no |

### 4.2 Cómo ven Packway los LLMs hoy

Perplexity/Google AI ya indexan la URL de **Azure** con el contenido de Packway. Eso significa:

1. Las citaciones pueden apuntar a `azurewebsites.net`, no a `packway.us`
2. La entidad "Packway" puede confundirse con el ERP Packway de BBI (producto más amplio)
3. Sin schema, los LLMs inferirán relación BBI ↔ Packway PSS ↔ Aurum solo del texto
4. El teléfono compartido con Aurum puede hacer que LLMs agrupen ambas marcas

### 4.3 Recomendaciones GEO

| Prioridad | Acción |
|-----------|--------|
| Alta | Crear `llms.txt` con definición clara: "Packway Pre-Sales Suite (PSS) is a B2B software product by BBI International for packaging converters…" |
| Alta | Añadir JSON-LD: `Organization` (BBI), `SoftwareApplication` (PSS), `WebSite` |
| Alta | Consolidar URL canónica en packway.us |
| Media | FAQ schema en sección "Typical Pre-Sales Challenges" |
| Media | Página `/about` con relación explícita BBI → PSS → Aurum (partner) |
| Media | Diferenciar entidad de "Packway ERP" (suite completa BBI) vs "Packway Pre-Sales Suite" |
| Baja | Publicar case studies / use cases indexables para citaciones long-tail |

### 4.4 Borrador sugerido para llms.txt

```txt
# Packway Pre-Sales Suite

> B2B software for packaging converters (corrugated, folding carton, displays).
> Developed by BBI International. Partner: Aurum Intelligence.

## Product
- Packway Estimating: instant quoting, ArtiosCAD integration, RFQ capture
- Docupoint: pre-sales CRM, project tracking, win/loss dashboard
- Integration Layer: JSON API for ERP/CRM connectivity

## Official site
https://packway.us/

## Related
- Developer: https://bbinternational.com/en/digital-formula/packway-eng/
- Partner: https://aurumintelligence.net/
```

---

## 5. Comparativa con ecosistema

| Sitio | Rol | Indexación |
|-------|-----|------------|
| packway.us | Landing US del Pre-Sales Suite | ❌ No indexado |
| azurewebsites.net/... | Hosting real expuesto | ✅ Aparece en Google |
| bbinternational.com/.../packway-eng/ | Documentación producto BBI | ✅ Indexado |
| aurumintelligence.net | Consultoría partner | ❌ No indexado (auditoría previa) |

**Riesgo:** Tres dominios compitiendo/confundiendo la entidad "Packway" sin canonical cruzado ni schema `isPartOf`.

---

## 6. Checklist de prioridades

### Urgente (esta semana)

- [ ] Corregir redirect `www.packway.us` → `https://packway.us/` (no Azure)
- [ ] Bloquear indexación de URL Azure (`robots.txt` en Azure o auth)
- [ ] Crear `robots.txt` y `sitemap.xml` estáticos
- [ ] Añadir `rel="canonical"` apuntando a `https://packway.us/`
- [ ] Registrar dominio en Google Search Console
- [ ] Quitar placeholders "Solution Title 5/6" o completar contenido

### Corto plazo (2–4 semanas)

- [ ] Open Graph + Twitter Cards (title, description, image 1200×630)
- [ ] Schema JSON-LD: Organization, SoftwareApplication, WebSite
- [ ] Crear `llms.txt`
- [ ] Favicon real (`favicon.ico`, `apple-touch-icon`)
- [ ] Privacy Policy y Terms (requerido para formulario B2B)
- [ ] Verificar formulario de contacto (destino, spam protection, GDPR)
- [ ] Corregir enlace Aurum → `aurumintelligence.net` si es el dominio correcto
- [ ] Alinear mensaje geográfico (.us vs Europe)

### Medio plazo (1–3 meses)

- [ ] Landing pages por módulo con URLs propias
- [ ] Case studies / customer logos con páginas dedicadas
- [ ] Blog o resource center (SEO informacional)
- [ ] Core Web Vitals — optimizar bundle Next.js (~120 KB HTML + chunks)
- [ ] Migrar assets de Vercel Blob a CDN propio o Azure Blob
- [ ] Estrategia de link building desde BBI hacia packway.us

---

## 7. URLs de referencia

| Recurso | URL | Estado |
|---------|-----|--------|
| Homepage | https://packway.us/ | ✅ OK |
| www (incorrecto) | https://www.packway.us/ | ❌ → Azure |
| Azure (expuesto) | https://app-pss-landing-page-c7b2eye9b7e2ajbt.canadacentral-01.azurewebsites.net/ | ⚠️ Indexable |
| BBI Packway | https://bbinternational.com/en/digital-formula/packway-eng/ | ✅ Referencia producto |
| robots.txt | https://packway.us/robots.txt | ❌ Devuelve HTML |
| sitemap.xml | https://packway.us/sitemap.xml | ❌ Devuelve HTML |

---

## 8. Notas del auditor

- Auditoría realizada mediante revisión de HTML, headers HTTP, redirects, búsqueda `site:` y resultados en motores generativos.
- Sitio construido con **v0.app** + Next.js export estático — típico de MVPs rápidos sin SEO técnico configurado.
- No se ejecutó Lighthouse; recomendable complementar con PageSpeed Insights.
- El contenido de producto es sólido para una landing; el cuello de botella es **infraestructura y señales técnicas**, no copy.
- Relacionado: auditoría previa de [Aurum Intelligence](../aurum-intelligence/auditorias/2026-06-10-seo-geo-inicial.md) — comparten teléfono y copy de contacto.

---

*Próxima revisión recomendada: 30 días después de corregir redirects y archivos técnicos.*
