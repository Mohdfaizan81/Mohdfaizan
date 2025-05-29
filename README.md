<!DOCTYPE html><html lang="en"><head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Faizan's Community</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.8.1/firebase-app.js";
    import {
      getFirestore,
      collection,
      addDoc,
      getDocs,
      query,
      where,
      orderBy,
      onSnapshot
    } from "https://www.gstatic.com/firebasejs/11.8.1/firebase-firestore.js";import {
  getAuth,
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  onAuthStateChanged,
  signOut
} from "https://www.gstatic.com/firebasejs/11.8.1/firebase-auth.js";

const firebaseConfig = {
  apiKey: "AIzaSyCpC99_Qpgdn2ZQqYOia6oUCpSAfam65vk",
  authDomain: "community-47319.firebaseapp.com",
  projectId: "community-47319",
  storageBucket: "community-47319.appspot.com",
  messagingSenderId: "259345994575",
  appId: "1:259345994575:web:29ecfb8dd4ca6938f92b3a",
  measurementId: "G-ST9GPQ51YM"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const auth = getAuth();

const msgInput = document.getElementById("messageInput");
const postBtn = document.getElementById("postBtn");
const msgList = document.getElementById("messages");

postBtn.addEventListener("click", async () => {
  const message = msgInput.value.trim();
  if (message !== "") {
    await addDoc(collection(db, "posts"), {
      message,
      createdAt: new Date().toISOString(),
      user: auth.currentUser.email
    });
    msgInput.value = "";
  }
});

onAuthStateChanged(auth, user => {
  if (user) {
    const q = query(collection(db, "posts"), orderBy("createdAt", "desc"));
    onSnapshot(q, snapshot => {
      msgList.innerHTML = "";
      snapshot.forEach(doc => {
        const data = doc.data();
        const div = document.createElement("div");
        div.className = "bg-white shadow-md rounded px-4 py-2 mb-2";
        div.innerHTML = `<strong>${data.user}</strong>: ${data.message}`;
        msgList.appendChild(div);
      });
    });
  }
});

  </script>
</head><body class="bg-gray-100 min-h-screen">
  <div class="max-w-xl mx-auto p-4">
    <h1 class="text-3xl font-bold text-center mb-6">Faizan's Community</h1>
    <div class="mb-4">
      <textarea id="messageInput" placeholder="Share something..." class="w-full p-3 rounded border border-gray-300"></textarea>
      <button id="postBtn" class="bg-blue-500 text-white px-4 py-2 mt-2 rounded">Post</button>
    </div>
    <div id="messages" class="mt-6"></div>
  </div>
</body></html>
