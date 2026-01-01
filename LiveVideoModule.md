# **Live Video Module – Technical & Execution Specification**

## **1\. Overview**

**Module Name:** Live Video Streaming Module  
**Tech Stack:**

* Frontend: Flutter  
* Backend: Node.js  
* Live Streaming: Agora  
* Database: MongoDB  
* Real-time Communication: WebSocket / Socket.IO

**Objective:**  
Enable a live video streaming feature where a host can start a live session, guests can join with video (limited), and audiences can watch in real time. The host has full moderation control.

---

| 2\. Roles & Permissions |  |  |  |
| :---- | :---- | :---- | :---- |
| **Feature** | **Host** | **Guest** | **Audience** |
| Start Live | ✅ | ❌ | ❌ |
| Join Video | ❌ | ✅ | ❌ |
| Camera ON/OFF | ❌ | ✅ | ❌ |
| Mic Mute/Unmute | ✅ | ✅ | ❌ |
| Kick / Mute / Block User | ✅ | ❌ | ❌ |
| Watch Live | ❌ | ❌ | ✅ |

---

## **3\. Live Video Rules (Important)**

* Only **Host** can start and end live  
* Maximum **4 video participants**  
  * 1 Host  
  * 3 Guests  
* Host camera:  
  * Auto start ( with front camera and can choose camera option before start live)   
  * No camera OFF  
  * No camera switch  
* Guest camera:  
  * ON/OFF allowed  
* Audience:  
  * Watch only  
  * No audio/video permissions but can comment and reactions

---

## **4\. User Flow**

### **4.1 Host Flow**

1. Host taps “Start Live”  
2. Backend creates live room  
3. Agora token generated (Broadcaster role)  
4. Host video auto-starts  
5. Host will manages guests (kick/mute/block)  
6. Host will ends live

---

### **4.2 Guest Flow**

1. Guest taps “Join Live”  
2. Backend validates:  
   * Guest limit not exceeded (max 3 guest)  
   * User not blocked or kicked  
3. Agora token generated (Broadcaster role)  
4. Guest joins video grid  
5. Guest can toggle camera & mic

---

### **4.3 Audience Flow**

1. Audience taps “Watch Live”  
2. Backend generates Agora token (Audience role)  
3. User watches live without controls

---

## **5\. Frontend Scope (Flutter)**

### **5.1 Screens**

* Host Live Screen  
* Guest Join Screen  
* Audience Watch Screen

---

### **5.2 Host Live Screen Features**

* Auto-start video  
* Mic mute/unmute  
* Guest list  
* Kick guest  
* Mute guest  
* Block user  
* No camera off or switch options

---

### **5.3 Guest Join Screen Features**

* Join live video  
* Camera ON/OFF  
* Mic mute/unmute

---

### **5.4 Video Layout**

* Grid layout  
* Max 4 video tiles  
* Responsive for different screen sizes  
* Dynamic update on user join/leave

---

### **5.5 Camera Filter**

* Agora beauty filter  
* Basic intensity control

---

### **5.6 Real-Time Event Handling (Socket)**

Flutter should handle:

* User joined  
* User left  
* User muted  
* User kicked  
* Live ended

---

### **5.7 API Integration (Flutter)**

* Join live  
* Leave live  
* Kick user  
* Mute user  
* Block user

---

## **6\. Backend Scope (Node.js)**

### **6.1 Live Room Management**

* Create live (host only)  
* End live  
* Track live status (active / ended)  
* Track active users

---

### **6.2 Agora Token Service**

* Role-based token generation  
  * Host → Broadcaster  
  * Guest → Broadcaster  
  * Audience → Audience  
* Token expiry handling

---

### **6.3 Join Validation Rules**

* Maximum 3 guests allowed  
* Blocked users cannot join  
* Kicked users cannot rejoin  
* Audience allowed unlimited (watch-only)

---

### **6.4 Host Control APIs**

* Kick guest  
* Mute guest  
* Block user  
  (All APIs secured — host only)

---

### **6.5 Real-Time Events (Socket)**

Backend emits:

* `user_joined`  
* `user_left`  
* `user_kicked`  
* `user_muted`  
* `live_ended`

---

### **6.6 Security & Permissions**

* Role-based access validation  
* Host-only moderation actions  
* Token verification  
* Prevent role escalation

---

## **7\. Database Structure (High-Level)**

* Live Room Collection  
* Active Users Collection  
* Blocked Users Collection  
* User Roles & Status

---

## **8\. Development Plan (Execution Order)**

**Phase 1:** Backend live room APIs & Agora token service  
**Phase 2:** Socket events & real-time sync  
**Phase 3:** Flutter UI screens & layouts  
**Phase 4:** Flutter–Backend integration

**Lastly :** Testing, edge cases & QA

---

## **9\. Risks & Considerations (we will handle this too for better user experience)**

* Agora token expiry & refresh  
* Network disconnection handling  
* Socket reconnection sync  
* App background/foreground behavior  
* Max participant enforcement consistency

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAQCAYAAAAWGF8bAAAAx0lEQVR4Xu2TYRHCMAyFKwEJSEBCjyVpXIAEHIATJCBhEpCAhEkA0tEtTVcod/zku8ufvDR7fduc+/NTmHkNge6t1QU82x0TQHRMg8i4t7rGI266QJc0b3Xt7Gq1d3jvV69zQyZUn9QAMHg5K8vnpuTBtFNzXzEawj5rKL0AmE7pFku3KXrR8jPHeaRELy00m2NhuYIstT1hPB8OqoF9dKmDbQQD3ZZcTznIJ2S1GmlZ5k4DhENa3FrbDz+Bk5eTIqhVdFbJ8wG0lJX5M/zhmwAAAABJRU5ErkJggg==>
