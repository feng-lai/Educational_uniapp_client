
# 🎓 Educational UniApp Client

This is a mobile client for an educational platform developed using **UniApp**, a cross-platform front-end framework powered by Vue.js. It provides a seamless user experience for students and teachers to access educational content, manage coursework, and engage with learning materials.

> 🔗 Paired with a backend API (not included in this repo), this client serves as the student-facing mobile portal.

## 📱 Features

- 📚 **Course List & Enrollment**
  - Browse available courses
  - Enroll in or quit courses
  - Course detail pages with video and material support

- 📝 **Homework & Assignments**
  - View assignments
  - Submit answers
  - View teacher feedback and scores

- 🧪 **Quizzes & Exams**
  - Multiple choice / subjective questions
  - Real-time grading and feedback

- 🗓 **Learning Progress**
  - Personal dashboard with completed and pending tasks
  - Course completion statistics

- 💬 **Messaging & Notifications**
  - Receive reminders for upcoming deadlines
  - Teacher-student communication module (if enabled)

- 👤 **User Center**
  - Edit profile, change password
  - View academic records and progress reports

## 🛠️ Tech Stack

- **Framework:** UniApp (Vue.js-based)
- **UI Library:** uView / custom components
- **Routing:** UniApp native routing
- **Storage:** Vuex + localStorage
- **Build Targets:** H5, WeChat Mini Program, Android/iOS via HBuilderX

## 🚀 Getting Started

### Prerequisites

- [HBuilderX](https://www.dcloud.io/hbuilderx.html) (recommended IDE for UniApp)
- Node.js & npm (if using CLI build)
- Backend API (not included)

### Installation

1. Clone the project:

```bash
git clone https://github.com/feng-lai/Educational_uniapp_client.git
cd Educational_uniapp_client
```

2. Open the project using **HBuilderX** or your preferred Vue-compatible editor.

3. Configure API endpoints:

   * Update API base URL in `common/request.js` or similar config file.

4. Run the app on your target platform:

   * HBuilderX: Click "Run to Browser / Mini Program / Emulator"
   * CLI: `npm run dev:%platform%` (e.g., `dev:h5`, `dev:mp-weixin`)

## 📁 Project Structure

```
Educational_uniapp_client/
├── pages/              # All page-level views
├── components/         # Reusable UI components
├── store/              # Vuex store for state management
├── static/             # Static resources (images, icons)
├── common/             # Utility functions, request config
├── uni.scss            # Global styles
└── main.js             # Entry point
```

## ⚙️ Configuration Tips

* ✅ Ensure API server supports CORS if testing on H5
* 🔒 Use token-based authentication (JWT recommended)
* 🌐 Multi-language support can be added using i18n plugins

## 📄 License

This project is licensed under the MIT License.
You are free to modify and use it in personal or commercial projects with attribution.

## 🙋 Author

Developed and maintained by [feng-lai](https://github.com/feng-lai)

---

> 📢 *Pull requests and feedback are welcome! If you’re building an educational app, this can be a great starting point for your mobile client.*


