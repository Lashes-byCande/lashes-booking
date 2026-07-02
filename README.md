[README.md](https://github.com/user-attachments/files/29613617/README.md)
# 🌸 Lashes by Cande — Guía de instalación

## Archivos del proyecto
- `index.html` — Página pública de reseñas
- `admin.html` — Panel de administración
- `README.md` — Esta guía

---

## PASO 1 — Crear cuenta Supabase (gratis)

1. Andá a [supabase.com](https://supabase.com) y creá una cuenta gratuita
2. Creá un nuevo proyecto (elegí la región más cercana, ej: South America)
3. Anotá:
   - **Project URL**: `https://xxxxx.supabase.co`
   - **Anon public key**: la encontrás en Settings → API

---

## PASO 2 — Crear las tablas en Supabase

Andá a **SQL Editor** en Supabase y ejecutá esto:

```sql
-- Tabla de reseñas
CREATE TABLE reviews (
  id          BIGSERIAL PRIMARY KEY,
  avatar      TEXT DEFAULT '🌸',
  rating      INT NOT NULL CHECK (rating BETWEEN 1 AND 5),
  comment     TEXT NOT NULL,
  visible     BOOLEAN DEFAULT TRUE,
  featured    BOOLEAN DEFAULT FALSE,
  position    INT DEFAULT 9999,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla de avatares
CREATE TABLE avatars (
  id        BIGSERIAL PRIMARY KEY,
  emoji     TEXT NOT NULL,
  active    BOOLEAN DEFAULT TRUE,
  position  INT DEFAULT 0
);

-- Insertar avatares por defecto
INSERT INTO avatars (emoji, position) VALUES
('🌸',0),('✨',1),('💖',2),('🦋',3),('🌷',4),
('☁️',5),('🌙',6),('🌺',7),('💫',8),('🌿',9),
('🎀',10),('🩷',11),('⭐',12),('🕊️',13),('🌼',14);

-- Permisos públicos (lectura de reseñas visibles y avatares activos)
ALTER TABLE reviews ENABLE ROW LEVEL SECURITY;
ALTER TABLE avatars ENABLE ROW LEVEL SECURITY;

-- Política: cualquiera puede leer reseñas visibles
CREATE POLICY "read_visible_reviews" ON reviews FOR SELECT USING (visible = TRUE);

-- Política: cualquiera puede insertar reseñas
CREATE POLICY "insert_reviews" ON reviews FOR INSERT WITH CHECK (TRUE);

-- Política: cualquiera puede leer avatares activos
CREATE POLICY "read_active_avatars" ON avatars FOR SELECT USING (active = TRUE);

-- Política: admin puede hacer todo (usamos service role en el admin)
-- Para el panel admin, temporalmente activar estas políticas:
CREATE POLICY "admin_all_reviews" ON reviews FOR ALL USING (TRUE);
CREATE POLICY "admin_all_avatars" ON avatars FOR ALL USING (TRUE);
```

> ⚠️ La última política (`admin_all_*`) es para pruebas. En producción, usá autenticación de Supabase para el admin.

---

## PASO 3 — Configurar los archivos

Abrí **index.html** y **admin.html** y reemplazá estas líneas al comienzo del script:

```javascript
const SUPABASE_URL = 'https://TU_PROJECT_ID.supabase.co';
const SUPABASE_ANON_KEY = 'TU_ANON_KEY_AQUI';
```

Con tus valores reales de Supabase.

---

## PASO 4 — Publicar en Netlify (gratis)

1. Andá a [netlify.com](https://netlify.com) y creá una cuenta
2. Arrastrá la carpeta del proyecto a **netlify.com/drop**
3. ¡Listo! Vas a obtener una URL como `https://lashes-by-cande.netlify.app`

**O desde GitHub:**
1. Subí los archivos a un repositorio de GitHub
2. En Netlify: New site → Import from Git → elegí tu repo
3. Build command: (vacío) / Publish directory: `.`

---

## Acceso al panel admin

- URL: `tu-sitio.netlify.app/admin.html`
- Usuario: `lashess.bycande@hotmail.com`
- Contraseña: `46040071`

> Para cambiar la contraseña, editá las líneas en `admin.html`:
> ```javascript
> const ADMIN_EMAIL = 'lashess.bycande@hotmail.com';
> const ADMIN_PASS  = '46040071';
> ```

---

## Funcionalidades incluidas ✅

**Página pública:**
- ✅ Reseña anónima con estrellas (1–5) y comentario
- ✅ Selección de avatar o asignación aleatoria
- ✅ Promedio de estrellas y total de reseñas
- ✅ Tarjetas elegantes con animaciones
- ✅ Anti-spam (30 segundos entre envíos)
- ✅ 100% responsive y mobile-first

**Panel admin:**
- ✅ Login protegido con usuario/contraseña
- ✅ Ver, ocultar/mostrar y eliminar reseñas
- ✅ Destacar reseñas importantes
- ✅ Filtrar por cantidad de estrellas
- ✅ Reordenar con drag & drop
- ✅ Estadísticas (total, promedio, visibles, ocultas)
- ✅ Gestión de avatares (agregar, desactivar, eliminar)
