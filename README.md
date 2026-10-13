# MAD Practical-7 – Login and Registration Screens using Jetpack Compose

## 📱 Jetpack Compose UI: Authentication Flow

### 🎯 AIM

Create an Android application that includes **Login** and **Registration** screens using **Jetpack Compose**.

---

## 📌 Objectives

This practical demonstrates:

- Building modern declarative UIs using **Jetpack Compose**
- Managing screen states using `remember` and `mutableStateOf`
- Input field handling via `OutlinedTextField`
- Layout composables: `Column`, `Row`, `Box`, and `Card`
- Styling and UI customization with `Modifier` (padding, sizing, shapes, elevation)
- Screen-to-screen navigation using Jetpack Compose Navigation (`NavHost`, `NavController`)
- Light and Dark theme adaptation with Material 3 styling
- Handling secure input (password visual transformations)

---

## 📚 Concepts Covered

The following core Android & Jetpack Compose concepts are covered:

- **Composable Functions (`@Composable`):** Building declarative UI components
- **State Management:** Tracking form inputs (`email`, `password`, `name`, `phone`, `city`) using `remember { mutableStateOf("") }`
- **Navigation Component:** Navigating between `LoginPage` and `RegisterPage`
- **Material 3 Components:** `Card`, `Button`, `Text`, `OutlinedTextField`
- **Modifiers:** Customizing alignment, spacing, fill dimensions, and rounded borders
- **Visual Transformation:** `PasswordVisualTransformation()` to mask sensitive input fields
- **Resource Loading:** Loading drawables such as `guni_pink_logo.png` via `painterResource()`

---

# 🚀 Application Flow

```text
               START
                 │
                 ▼
          AppNavigation
                 │
                 ▼
           [ LoginPage ]
           ├── GUNI Logo
           ├── Card Container
           │   ├── Email Field
           │   ├── Password Field
           │   ├── "Forgot Password?"
           │   └── Login Button
           └── "Don't have an account? SIGN UP" ──(Navigate)──┐
                                                              │
                                                              ▼
                                                      [ RegisterPage ]
                                                      ├── GUNI Logo
                                                      ├── Form Fields (Name, Phone, City, Email, Password, Confirm)
                                                      ├── Register Button
                                                      └── "Do you have an account? LOGIN" ──(Back to Login)

```

---

# 🧩 Screen Architecture

### 1. `LoginPage.kt`

* Displays the brand header (**GUNI**).
* Features an elevated form card containing:
* **Email** input field
* **Password** input field (hidden with password mask)
* Interactive text for **Forgot Password?**
* Primary **Login** button


* Bottom navigation link: **"Don't have an account? SIGN UP"** directs the user to `RegisterPage`.

### 2. `RegisterPage.kt`

* Form container with fields:
* **Name**
* **Phone Number**
* **City**
* **Email**
* **Password**
* **Confirm Password**


* Primary **Register** action button.
* Bottom navigation link: **"Do you have an account? LOGIN"** pops back to `LoginPage`.

### 3. `AppNavigation.kt`

Controls the navigation graph via `NavHost`:

```kotlin
sealed class Screen(val route: String) {
    object Login : Screen("login")
    object Register : Screen("register")
}

@Composable
fun AppNavigation(navController: NavHostController = rememberNavController()) {
    NavHost(
        navController = navController,
        startDestination = Screen.Login.route
    ) {
        composable(Screen.Login.route) {
            LoginPage(
                onNavigateToRegister = { navController.navigate(Screen.Register.route) }
            )
        }
        composable(Screen.Register.route) {
            RegisterPage(
                onNavigateToLogin = { navController.popBackStack() }
            )
        }
    }
}

```

---

# 🛠️ Key Implementation Snippets

### State Handling for Form Inputs

```kotlin
var email by remember { mutableStateOf("") }
var password by remember { mutableStateOf("") }

OutlinedTextField(
    value = email,
    onValueChange = { email = it },
    label = { Text("Enter Email") },
    singleLine = true,
    modifier = Modifier.fillMaxWidth()
)

OutlinedTextField(
    value = password,
    onValueChange = { password = it },
    label = { Text("Enter Password") },
    visualTransformation = PasswordVisualTransformation(),
    singleLine = true,
    modifier = Modifier.fillMaxWidth()
)

```

### Elevated Card Form Wrapper

```kotlin
Card(
    shape = RoundedCornerShape(16.dp),
    elevation = CardDefaults.cardElevation(defaultElevation = 8.dp),
    modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp)
) {
    Column(
        modifier = Modifier.padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // Form Fields and Buttons
    }
}

```

---

# 📂 Project Structure

```text
24012021020_MAD_Practical7/
│
├── app/
│   └── src/
│       └── main/
│           ├── java/com/example/a24012021020_mad_practical7/
│           │   ├── screen/
│           │   │   ├── LoginPage.kt
│           │   │   └── RegisterPage.kt
│           │   │
│           │   ├── ui/theme/
│           │   │   ├── Color.kt
│           │   │   ├── Theme.kt
│           │   │   └── Type.kt
│           │   │
│           │   ├── AppNavigation.kt
│           │   └── MainActivity.kt
│           │
│           └── res/
│               ├── drawable/
│               │   └── guni_pink_logo.png
│               └── values/
│                   ├── colors.xml
│                   └── strings.xml
│
├── build.gradle.kts
└── README.md

```

---

# ▶️ How to Run

1. Clone or download this project.
2. Open the project in **Android Studio**.
3. Allow Gradle to download dependencies and sync successfully.
4. Select an active Android Virtual Device (AVD) or connect a physical device with USB debugging enabled.
5. Click **Run ▶ (Shift + F10)**.
6. The app opens directly to `LoginPage`.
7. Tap **SIGN UP** to test navigation to `RegisterPage`.
8. Tap **LOGIN** at the bottom of the registration screen to navigate back.

---

# 🧪 Expected Output

| Screen | Description |
| --- | --- |
| **Login Screen** | Displays the GUNI header, email/password text fields inside an elevated card, login button, and link to Register screen. |
| **Register Screen** | Displays full registration form (Name, Phone, City, Email, Password, Confirm Password) and navigation link back to Login. |
| **Theme Adaptability** | Correctly maps colors across both Light Mode and Dark Mode backgrounds. |

---

# 📚 Important Viva Questions

### 1. What is Jetpack Compose?

Jetpack Compose is Android’s modern, declarative UI toolkit. It simplifies and accelerates UI development with less code, intuitive Kotlin APIs, and dynamic reactivity without relying on XML layout files.

### 2. What is the difference between Imperative UI and Declarative UI?

In imperative UI (XML/Views), you manually mutate views using setters (e.g., `textView.setText(...)`). In declarative UI (Compose), you describe what the UI should look like for a given state; when state updates, the UI automatically recomposes.

### 3. What is Recomposition?

Recomposition is the process of re-calling composable functions when their underlying state data changes, allowing the UI to reflect new information without manually mutating view objects.

### 4. What is the purpose of `remember` and `mutableStateOf`?

* `mutableStateOf` creates an observable state holder that Compose tracks for recomposition.
* `remember` preserves that state across recompositions so values are not reset when the composable function re-executes.

### 5. How does navigation work in Jetpack Compose?

Jetpack Compose uses `NavHostController` and `NavHost` where each destination is mapped using a route identifier string or type-safe object.

### 6. What is `PasswordVisualTransformation`?

It is a built-in transformation class in Jetpack Compose used with text fields to mask characters (e.g., replacing passwords with asterisks or dots).

---

## 👨‍💻 Author

**Yug Jivani**

**Enrollment No.:** `24012021020`

**Course:** B.Tech Information Technology

**Practical:** MAD Practical-7

```

```
