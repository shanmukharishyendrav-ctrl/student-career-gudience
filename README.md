import React, { useState } from 'react'
import { BrowserRouter as Router, Routes, Route, useNavigate } from 'react-router-dom'
import './App.css'
import './styles/Common.css'
import Login from './pages/Login'
import SignUp from './pages/SignUp'
import AdminDashboard from './pages/AdminDashboard'
import UserDashboard from './pages/UserDashboard'
import CareerPaths from './pages/CareerPaths'
import CounsellorList from './pages/CounsellorList'
import BookSession from './pages/BookSession'
import AdminResources from './pages/AdminResources'
import AdminCounsellors from './pages/AdminCounsellors'
import UserProfile from './pages/UserProfile'
import Navbar from './components/Navbar'

function App() {
  const [user, setUser] = useState(null)
  const [role, setRole] = useState(null)

  const handleLogin = (userData, userRole) => {
    setUser(userData)
    setRole(userRole)
  }

  const handleLogout = () => {
    setUser(null)
    setRole(null)
  }

  return (
    <Router>
      <div className="app-container">
        {user && <Navbar user={user} role={role} onLogout={handleLogout} />}
        <Routes>
          <Route path="/" element={user ? (role === 'admin' ? <AdminDashboard user={user} /> : <UserDashboard user={user} />) : <Login onLogin={handleLogin} />} />
          <Route path="/login" element={<Login onLogin={handleLogin} />} />
          <Route path="/signup" element={<SignUp onLogin={handleLogin} />} />
          
          {/* User Routes */}
          <Route path="/user/dashboard" element={user && role === 'user' ? <UserDashboard user={user} /> : <Login onLogin={handleLogin} />} />
          <Route path="/user/career-paths" element={user && role === 'user' ? <CareerPaths /> : <Login onLogin={handleLogin} />} />
          <Route path="/user/counsellors" element={user && role === 'user' ? <CounsellorList /> : <Login onLogin={handleLogin} />} />
          <Route path="/user/book-session" element={user && role === 'user' ? <BookSession user={user} /> : <Login onLogin={handleLogin} />} />
          <Route path="/user/profile" element={user && role === 'user' ? <UserProfile user={user} setUser={setUser} /> : <Login onLogin={handleLogin} />} />
          
          {/* Admin Routes */}
          <Route path="/admin/dashboard" element={user && role === 'admin' ? <AdminDashboard user={user} /> : <Login onLogin={handleLogin} />} />
          <Route path="/admin/resources" element={user && role === 'admin' ? <AdminResources /> : <Login onLogin={handleLogin} />} />
          <Route path="/admin/counsellors" element={user && role === 'admin' ? <AdminCounsellors /> : <Login onLogin={handleLogin} />} />
        </Routes>
      </div>
    </Router>
  )
}

export default App
