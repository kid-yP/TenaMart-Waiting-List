TenaMart Waiting List Admin Dashboard
A simple and elegant admin dashboard built with Vue 3 and Tailwind CSS to manage and visualize users registered on TenaMart’s waiting list.

Features
User Cards: Displays registered users with details.

Search & Filtering: Filter users by name, email, and signup source.

Pagination: Navigate user lists easily with pagination controls.

CSV Export: Export filtered user data as CSV file.

Block/Unblock Users: Toggle user access with confirmation.

Delete Users: Remove users with confirmation prompt and smooth animations.

Signup Analytics: Visualize signups by day (line chart) and by source (bar chart).

Responsive UI: Fully responsive layout with Tailwind CSS.

Animations: Fade-scale transition effects for a polished experience.

Tech Stack
Vue 3

Vite

Tailwind CSS

Chart.js

PapaParse (CSV export)

Demo
(Include link to a live demo or screenshots here)

Installation
Clone the repository

bash
Copy
Edit
git clone https://github.com/your-username/tenamart-admin-dashboard.git
cd tenamart-admin-dashboard
Install dependencies

bash
Copy
Edit
npm install
Run the development server

bash
Copy
Edit
npm run dev
Open in browser

Navigate to http://localhost:5173 (or the port shown in your terminal).

Project Structure
bash
Copy
Edit
src/
 ├── components/
 │    ├── UserCard.vue        # User card display component
 │    ├── SearchBar.vue       # Search and filter component
 │    └── SignupChart.vue     # Charts for signup analytics
 ├── data/
 │    └── users.js            # Mock user data
 ├── views/
 │    └── Dashboard.vue       # Main dashboard view
 ├── App.vue
 └── main.js                  # Entry point
Usage
Use the search bar to filter users by name, email, or signup source.

Click Download CSV to export the current filtered list.

Use Block/Unblock button on user cards to toggle user status (with confirmation).

Use Delete button on user cards to remove a user (with confirmation and animation).

View signup trends in the charts below the user list.

Navigate pages with the pagination controls.

Customization
Branding: Customize colors and fonts by editing Tailwind config and component styles.

Data: Replace src/data/users.js with your backend API or Firestore integration.

Pagination: Adjust pageSize in Dashboard.vue to change items per page.

Future Improvements
Connect to a real backend for persistent data.

Add infinite scrolling option.

Add user roles and permissions.

Enhance accessibility features.

Add more charts and analytics.
