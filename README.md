# TenaMart Waiting List Admin Dashboard

An elegant, responsive dashboard built with **Vue 3** and **Tailwind CSS** to manage, monitor, and analyze user registrations for TenaMart’s waiting list.

## 🌟 Features

🧍 **User Cards** — Display user details with status tags
  
🔍 **Search & Filter** — Real-time filtering by name, email, or signup source
  
📄 **CSV Export** — Export filtered user data for analysis or storage
  
🚫 **Block/Unblock Users** — Toggle user access with confirmation
  
🗑️ **Delete Users** — Smooth deletion animations with confirmation modals
  
📊 **Signup Analytics** — Line chart by date + bar chart by source
  
🧭 **Pagination** — Navigate large user sets with ease
  
🎨 **Responsive UI** — Fluid layout optimized for all screen sizes
 
✨ **Animations** — Fade-scale transitions for a refined UX

## 🛠️ Tech Stack

| Tool         | Role                             |
|--------------|----------------------------------|
| Vue 3        | Frontend framework               |
| Vite         | Dev server & build tool          |
| Tailwind CSS | Utility-first styling            |
| Chart.js     | Data visualization (line/bar)    |
| PapaParse    | CSV generation for export        |

## 🔗 Demo

Live preview: [tena-mart-waiting-list.vercel.app](https://tena-mart-waiting-list.vercel.app)

## 🚀 Installation

```bash
# Clone the repository
git clone https://github.com/your-username/tenamart-admin-dashboard.git
cd tenamart-admin-dashboard

# Install dependencies
npm install

# Start development server
npm run dev

Visit http://localhost:5173 (or whatever port your terminal displays).

📁 Project Structure

src/
 ├── components/
 │    ├── AdminDashboard.vue    # Main dashboard component orchestrating the app
 │    ├── ConfirmModal.vue      # Reusable confirmation modal dialog
 │    ├── SearchBar.vue         # Search and filter UI component
 │    ├── SignupChart.vue       # Charts component visualizing signup analytics
 │    └── UserCard.vue          # Card component displaying individual user info
 │
 ├── data/
 │    └── users.js              # Mock user data for development/testing
 │
 ├── views/
 │    └── Dashboard.vue         # Main page/view integrating all components
 │
 ├── App.vue                   # Root Vue component
 └── main.js                   # Application entry point and setup


📘 Usage Guide

Use the SearchBar to filter by name, email, or source

Export filtered results by clicking Download CSV

Toggle user status via Block/Unblock with confirmations

Remove users with Delete and smooth transition feedback

View analytics with charts by signup date and signup source

Navigate user data using built-in pagination controls

🎨 Customization Tips

Branding: Edit Tailwind theme for custom colors, fonts, breakpoints

Backend Integration: Replace users.js with live API or Firestore

Pagination Limit: Adjust pageSize value in Dashboard.vue

📈 Future Improvements

🔄 Infinite scroll alternative for pagination

🔗 Backend integration with CRUD support

🛡️ User roles and permission levels

♿ Accessibility enhancements (keyboard navigation, ARIA)

📊 Expanded analytics (device type, location, conversion trends)
