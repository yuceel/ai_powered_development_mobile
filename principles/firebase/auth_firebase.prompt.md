# Step-by-Step Firebase Authentication for Mobile Apps

Guide an AI to generate code and instructions for implementing Firebase Authentication in a mobile application. The steps should be clear, actionable, and suitable for both iOS and Android platforms. Include code snippets and cover additional Firebase Auth use cases and packages.

## 1. Firebase Project Setup
- Create a new project in the [Firebase Console](https://console.firebase.google.com/).
- Register your mobile app (Android/iOS) and download the configuration files (`google-services.json` for Android, `GoogleService-Info.plist` for iOS).
- Add the configuration files to your project as instructed by Firebase.

## 2. Add Firebase SDK and Packages
- Add the required Firebase dependencies to your project:
  - For Flutter:  
    ```yaml
    dependencies:
      firebase_core: ^latest
      firebase_auth: ^latest
      google_sign_in: ^latest
      sign_in_with_apple: ^latest
      cloud_firestore: ^latest
      flutter_secure_storage: ^latest
    ```
- Initialize Firebase in your app’s entry point before using any Firebase services.

  ```dart
  // main.dart
  import 'package:firebase_core/firebase_core.dart';

  void main() async {
    WidgetsFlutterBinding.ensureInitialized();
    await Firebase.initializeApp();
    runApp(MyApp());
  }
  ```

## 3. Enable Authentication Providers
- In the Firebase Console, navigate to Authentication > Sign-in method.
- Enable the desired authentication providers (Email/Password, Google, Apple, Phone, Anonymous).
- Configure provider-specific settings as needed (e.g., OAuth credentials for Google/Apple).

## 4. Implement Authentication Flows

### Email/Password Registration and Login

```dart
// Registration
await FirebaseAuth.instance.createUserWithEmailAndPassword(
  email: email,
  password: password,
);

// Login
await FirebaseAuth.instance.signInWithEmailAndPassword(
  email: email,
  password: password,
);
```

### Google Sign-In

```dart
final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();
final GoogleSignInAuthentication googleAuth = await googleUser!.authentication;
final credential = GoogleAuthProvider.credential(
  accessToken: googleAuth.accessToken,
  idToken: googleAuth.idToken,
);
await FirebaseAuth.instance.signInWithCredential(credential);
```

### Apple Sign-In (iOS)

```dart
final appleCredential = await SignInWithApple.getAppleIDCredential(
  scopes: [AppleIDAuthorizationScopes.email, AppleIDAuthorizationScopes.fullName],
);
final oauthCredential = OAuthProvider("apple.com").credential(
  idToken: appleCredential.identityToken,
  accessToken: appleCredential.authorizationCode,
);
await FirebaseAuth.instance.signInWithCredential(oauthCredential);
```

### Phone Authentication

```dart
await FirebaseAuth.instance.verifyPhoneNumber(
  phoneNumber: '+1234567890',
  verificationCompleted: (PhoneAuthCredential credential) async {
    await FirebaseAuth.instance.signInWithCredential(credential);
  },
  verificationFailed: (FirebaseAuthException e) { /* handle error */ },
  codeSent: (String verificationId, int? resendToken) { /* save verificationId */ },
  codeAutoRetrievalTimeout: (String verificationId) {},
);
```

### Password Reset

```dart
await FirebaseAuth.instance.sendPasswordResetEmail(email: email);
```

### Anonymous Authentication

```dart
await FirebaseAuth.instance.signInAnonymously();
```

- Display error messages and loading indicators for better UX.

## 5. Manage User State

- Listen to authentication state changes to update the UI:

```dart
FirebaseAuth.instance.authStateChanges().listen((User? user) {
  if (user == null) {
    // Show login screen
  } else {
    // Show home screen
  }
});
```

- Secure routes/screens by checking the user’s authentication status.

## 6. Sign Out and Account Management

- Sign out:

```dart
await FirebaseAuth.instance.signOut();
```

- Update user profile:

```dart
await FirebaseAuth.instance.currentUser?.updateDisplayName('New Name');
await FirebaseAuth.instance.currentUser?.updatePhotoURL('https://...');
```

- Account deletion:

```dart
await FirebaseAuth.instance.currentUser?.delete();
```

- Update email/password:

```dart
await FirebaseAuth.instance.currentUser?.updateEmail('new@email.com');
await FirebaseAuth.instance.currentUser?.updatePassword('newPassword');
```

## 7. Advanced Use Cases

- **Multi-factor Authentication:**  
  Use [firebase_auth](https://pub.dev/packages/firebase_auth) to enable multi-factor authentication for added security.
- **Linking Accounts:**  
  Link multiple sign-in methods to a single user account.
  ```dart
  final credential = GoogleAuthProvider.credential(...);
  await FirebaseAuth.instance.currentUser?.linkWithCredential(credential);
  ```
- **Re-authentication:**  
  Required for sensitive operations.
  ```dart
  final credential = EmailAuthProvider.credential(email: email, password: password);
  await FirebaseAuth.instance.currentUser?.reauthenticateWithCredential(credential);
  ```
- **Custom Claims:**  
  Use Firebase Admin SDK to set custom claims for role-based access.

## 8. Security and Best Practices

- Validate user input on both client and server sides.
- Use `flutter_secure_storage` for sensitive data (tokens, credentials).
- Set up Firebase Authentication security rules in the console.
- Log authentication events for analytics and debugging.
- Regularly update dependencies for security patches.

---

**Note:** All steps and code should be adapted to the specific mobile framework (e.g., Flutter, React Native, native Android/iOS) as needed. Always refer to the official Firebase documentation for the latest best practices and API changes.
