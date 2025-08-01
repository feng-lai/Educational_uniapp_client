
[English](README.md)  
[日本語](README-jp.md)  
[العربية](README-ar.md)  
[Português](README-pt.md)  

# 🎓 Cliente Educativo UniApp

Este es un cliente móvil para una plataforma educativa desarrollado con **UniApp**, un framework frontend multiplataforma basado en Vue.js. Ofrece una experiencia fluida para que estudiantes y profesores accedan al contenido educativo, gestionen tareas y participen en actividades de aprendizaje.

> 🔗 En conjunto con una API backend (no incluida en este repositorio), este cliente funciona como el portal móvil orientado al estudiante.

## 📱 Funcionalidades

- 📚 **Listado e Inscripción a Cursos**
  - Explora los cursos disponibles
  - Inscríbete o abandona cursos
  - Páginas de detalle de cursos con videos y materiales

- 📝 **Tareas y Deberes**
  - Ver tareas asignadas
  - Enviar respuestas
  - Revisar comentarios y calificaciones del profesor

- 🧪 **Pruebas y Exámenes**
  - Preguntas de opción múltiple y abiertas
  - Corrección automática y retroalimentación en tiempo real

- 🗓 **Progreso de Aprendizaje**
  - Panel personal con tareas completadas y pendientes
  - Estadísticas de finalización de cursos

- 💬 **Mensajería y Notificaciones**
  - Recibir recordatorios de fechas importantes
  - Módulo de comunicación profesor-alumno (si está habilitado)

- 👤 **Centro de Usuario**
  - Editar perfil, cambiar contraseña
  - Ver historial académico e informes de progreso

## 🛠️ Tecnologías Utilizadas

- **Framework:** UniApp (basado en Vue.js)  
- **Librería UI:** uView / componentes personalizados  
- **Enrutamiento:** Enrutamiento nativo de UniApp  
- **Almacenamiento:** Vuex + localStorage  
- **Plataformas de destino:** H5, Mini Program de WeChat, Android/iOS vía HBuilderX

## 🚀 Primeros Pasos

### Requisitos Previos

- [HBuilderX](https://www.dcloud.io/hbuilderx.html) (IDE recomendada para UniApp)
- Node.js y npm (si se usa compilación por CLI)
- API backend (no incluida)

### Instalación

1. Clona el proyecto:

```bash
git clone https://github.com/feng-lai/Educational_uniapp_client.git
cd Educational_uniapp_client
````

2. Abre el proyecto con **HBuilderX** o un editor compatible con Vue.

3. Configura los endpoints de la API:

   * Actualiza la URL base en `common/request.js` u otro archivo de configuración.

4. Ejecuta la aplicación en tu plataforma de destino:

   * HBuilderX: Haz clic en "Ejecutar en Navegador / Mini Program / Emulador"
   * CLI: `npm run dev:%platform%` (por ejemplo: `dev:h5`, `dev:mp-weixin`)

## 📁 Estructura del Proyecto

```
Educational_uniapp_client/
├── pages/              # Vistas de páginas
├── components/         # Componentes reutilizables
├── store/              # Gestión de estado con Vuex
├── static/             # Recursos estáticos (imágenes, íconos)
├── common/             # Funciones auxiliares, configuración de peticiones
├── uni.scss            # Estilos globales
└── main.js             # Punto de entrada
```

## ⚙️ Consejos de Configuración

* ✅ Asegúrate de que el servidor API soporte CORS si pruebas en H5
* 🔒 Usa autenticación basada en tokens (recomendado JWT)
* 🌐 Soporte multilenguaje puede integrarse con plugins de i18n

## 📄 Licencia

Este proyecto está licenciado bajo la Licencia MIT. Puedes modificarlo y utilizarlo en proyectos personales o comerciales con atribución.

## 🙋 Autor

Desarrollado y mantenido por [feng-lai](https://github.com/feng-lai)

---

> 📢 *¡Se agradecen los pull requests y sugerencias! Si estás desarrollando una app educativa, este proyecto puede ser un excelente punto de partida para tu cliente móvil.*

