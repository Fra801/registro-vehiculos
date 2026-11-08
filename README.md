# Registro de Vehículos — guía de instalación (todo gratis)

## 1. Planilla y backend
1. Creá una Google Sheet nueva (privada, no la compartas).
2. **Extensiones > Apps Script**. Borrá el contenido y pegá `Code.gs`.
3. Ejecutá la función `setup` (aceptá los permisos). Crea las pestañas `Vehiculos`, `Usuarios` y `Auditoria`.
4. En la pestaña **Usuarios** cargá los correos Gmail autorizados:

| email | nivel | activo |
|---|---|---|
| tucorreo@gmail.com | 1 | SI |
| otro@gmail.com | 2 | SI |
| consulta@gmail.com | 3 | SI |

   Nivel 1 = leer/agregar/modificar · Nivel 2 = leer/agregar · Nivel 3 = solo leer. Para quitar acceso: poné `NO`.

## 2. Client ID de Google (login)
1. Entrá a https://console.cloud.google.com y creá un proyecto.
2. **APIs y servicios > Pantalla de consentimiento OAuth**: tipo *Externo*, completá nombre y correo.
3. **Credenciales > Crear credenciales > ID de cliente de OAuth > Aplicación web**.
4. En *Orígenes autorizados de JavaScript* agregá `https://TU-USUARIO.github.io`.
5. Copiá el **Client ID** y pegalo en `Code.gs` (`CLIENT_ID`) y en `index.html` (`CLIENT_ID`).

## 3. Publicar el backend
En Apps Script: **Implementar > Nueva implementación > Aplicación web**
- Ejecutar como: **Yo**
- Quién tiene acceso: **Cualquier persona**

Copiá la URL (`.../exec`) y pegala en `index.html` (`API_URL`). Si cambiás `Code.gs`, hacé una *nueva versión* de la implementación.

> "Cualquier persona" solo abre la puerta de la API: sin un token de Google de un correo que esté en `Usuarios`, no devuelve nada.

## 4. Publicar en GitHub Pages
1. Creá un repositorio y subí `index.html`.
2. **Settings > Pages > Deploy from branch > main / root**.
3. Tu app queda en `https://TU-USUARIO.github.io/NOMBRE-REPO/`.
4. Si usás otra URL, agregala también en los orígenes autorizados del paso 2.

El repo no contiene datos ni claves secretas (el Client ID es público por diseño).

## Campos que guarda
- **Anverso:** dominio, marca, modelo, tipo, uso, chasis, motor.
- **Reverso:** apellido, nombres, documento, domicilio, localidad, departamento, provincia.
- **Opcionales:** año, color, observaciones.

## Carga por foto
Cada lado (anverso y reverso) se puede cargar con **Sacar foto** (cámara) o **Galería** (fotos ya sacadas). Los campos en amarillo conviene revisarlos. Si el chasis no cumple el dígito verificador, la app sugiere la corrección más probable.

## Actualizar el backend sin cambiar la URL
Apps Script > **Implementar > Gestionar implementaciones > lápiz > Versión: Nueva versión > Implementar**.

## Notas
- El OCR (Tesseract.js) corre en el teléfono: gratis y las fotos no se envían a nadie. La precisión es media; por eso siempre se revisa antes de guardar. Foto nítida, bien iluminada y de frente mejora mucho el resultado.
- Las fotos **no se guardan**, solo los datos.
- Cada lectura, alta y modificación queda en `Auditoria`.
- Son datos personales: aplica la Ley 25.326. Usá verificación en dos pasos en las cuentas y confirmá que la institución autorice el uso.
