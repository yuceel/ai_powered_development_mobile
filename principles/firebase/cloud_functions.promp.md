# Step-by-Step Firebase Cloud Functions Integration for Mobile Apps

Guide an AI to generate code and instructions for integrating Firebase Cloud Functions into a mobile application. Steps should be actionable, clear, and suitable for both iOS and Android platforms. Include code snippets, advanced use cases, and additional Firebase package integrations.

## 1. Set Up Firebase Cloud Functions

- Install Node.js and Firebase CLI if not already installed.
  ```sh
  npm install -g firebase-tools
  ```
- Authenticate and initialize Cloud Functions in your Firebase project:
  ```sh
  firebase login
  firebase init functions
  ```
- Choose JavaScript or TypeScript and install dependencies:
  ```sh
  cd functions
  npm install
  ```

## 2. Write and Deploy Cloud Functions

- Example: Create an HTTP function in `functions/index.js`:
  ```js
  // JavaScript example
  const functions = require('firebase-functions');
  exports.helloWorld = functions.https.onRequest((req, res) => {
    res.send("Hello from Firebase!");
  });
  ```
- Example: HTTPS Callable function:
  ```js
  exports.addNumbers = functions.https.onCall((data, context) => {
    const { a, b } = data;
    if (typeof a !== 'number' || typeof b !== 'number') {
      throw new functions.https.HttpsError('invalid-argument', 'Numbers required.');
    }
    return { sum: a + b };
  });
  ```
- Deploy your functions:
  ```sh
  firebase deploy --only functions
  ```

## 3. Enable Cloud Functions in Your Mobile App

- Add required Firebase packages:
  - For Flutter:
    ```yaml
    dependencies:
      cloud_functions: ^latest
      firebase_core: ^latest
      firebase_auth: ^latest
      cloud_firestore: ^latest
    ```
- Initialize Firebase in your app’s entry point:
  ```dart
  import 'package:firebase_core/firebase_core.dart';
  void main() async {
    WidgetsFlutterBinding.ensureInitialized();
    await Firebase.initializeApp();
    runApp(MyApp());
  }
  ```

## 4. Call Cloud Functions from Mobile

- Example: Calling an HTTPS Callable Function in Flutter:
  ```dart
  import 'package:cloud_functions/cloud_functions.dart';

  final HttpsCallable callable = FirebaseFunctions.instance.httpsCallable('helloWorld');
  final result = await callable();
  print(result.data); // Should print "Hello from Firebase!"
  ```

- Pass parameters to your function:
  ```dart
  final HttpsCallable callable = FirebaseFunctions.instance.httpsCallable('addNumbers');
  final result = await callable({'a': 3, 'b': 5});
  print(result.data['sum']); // Should print 8
  ```

- Example: Calling with authentication (user must be signed in):
  ```dart
  import 'package:firebase_auth/firebase_auth.dart';

  // Ensure user is signed in
  final user = FirebaseAuth.instance.currentUser;
  if (user != null) {
    final callable = FirebaseFunctions.instance.httpsCallable('secureFunction');
    final result = await callable({'param': 'value'});
    print(result.data);
  }
  ```

- Example: Using Cloud Functions with Firestore triggers (backend):
  ```js
  exports.onUserCreate = functions.firestore
    .document('users/{userId}')
    .onCreate((snap, context) => {
      // Perform some logic when a user is created
      return null;
    });
  ```

## 5. Handle Errors and Responses

- Use try-catch to handle errors:
  ```dart
  try {
    final result = await callable();
    // handle result
  } on FirebaseFunctionsException catch (e) {
    print('Cloud Function Error: ${e.code} - ${e.message}');
  } catch (e) {
    print('Unknown error: $e');
  }
  ```

- Handle custom error codes and messages from your function.

## 6. Secure Your Functions

- Use Firebase Authentication to restrict access:
  - In your callable function, check `context.auth` to verify the user is authenticated.
  ```js
  exports.secureFunction = functions.https.onCall((data, context) => {
    if (!context.auth) {
      throw new functions.https.HttpsError('unauthenticated', 'User must be signed in.');
    }
    // Secure logic here
    return { status: 'success' };
  });
  ```
- Validate input data and sanitize all parameters.
- Set up Firebase security rules for Firestore/Storage if your functions interact with them.

## 7. Advanced Use Cases

- **Firestore Triggers:** Run backend logic automatically when data changes.
  ```js
  exports.onMessageWrite = functions.firestore
    .document('messages/{messageId}')
    .onWrite((change, context) => {
      // Handle message creation or update
      return null;
    });
  ```
- **Auth Triggers:** Send welcome emails or set custom claims when a user signs up.
  ```js
  exports.sendWelcomeEmail = functions.auth.user().onCreate((user) => {
    // Send email logic
    return null;
  });
  ```
- **Storage Triggers:** Process images or files when uploaded.
  ```js
  exports.onFileUpload = functions.storage.object().onFinalize((object) => {
    // Image processing logic
    return null;
  });
  ```
- **Scheduling Functions:** Use `firebase-functions` with `pubsub` to run scheduled tasks.
  ```js
  exports.scheduledFunction = functions.pubsub.schedule('every 24 hours').onRun((context) => {
    // Scheduled logic
    return null;
  });
  ```
- **Send FCM Notifications:** Use functions to send push notifications via Firebase Cloud Messaging.

## 8. Best Practices

- Keep functions small and focused.
- Use environment configuration for secrets and API keys.
- Monitor function logs and errors in the Firebase Console.
- Use `firebase-functions-test` for unit testing your functions.
- Optimize cold start times by minimizing dependencies.

---

**Note:** Adapt code and steps to your specific mobile framework (Flutter, React Native, native Android/iOS) as needed. Always refer to the official Firebase documentation for updates and best practices.
