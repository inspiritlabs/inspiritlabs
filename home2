import { useState } from "react";
import { useLocation } from "wouter";
import { apiRequest } from "@/lib/queryClient";
import AccessCodeValidator from "@/components/access-code-validator";

export default function Home() {
  const [, setLocation] = useLocation();
  const [isSignIn, setIsSignIn] = useState(true);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [accessCode, setAccessCode] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState("");
  const [showAccessValidator, setShowAccessValidator] = useState(false);

  const handleAuth = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!email.trim() || !password.trim()) return;
    
    if (!isSignIn && !accessCode.trim()) {
      setShowAccessValidator(true);
      return;
    }
    
    setIsLoading(true);
    setError("");
    
    try {
      const endpoint = isSignIn ? "/api/auth/login" : "/api/auth/register";
      const body = isSignIn 
        ? { email, password }
        : { email, password, accessCode };
      
      const response = await fetch(endpoint, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(body),
      });

      if (response.ok) {
        const data = await response.json();
        localStorage.setItem("user", JSON.stringify(data.user));
        setLocation("/dashboard");
      } else {
        const errorData = await response.json();
        setError(errorData.error || "Authentication failed");
      }
    } catch (err) {
      setError("Network error. Please try again.");
    } finally {
      setIsLoading(false);
    }
  };

  const handleValidCode = (code: string) => {
    setAccessCode(code);
    setShowAccessValidator(false);
  };

  if (showAccessValidator) {
    return <AccessCodeValidator onValidCode={handleValidCode} />;
  }

  return (
    <div className="auth-wrapper">
      <div className="gradient-bg" />
      <div className="dust-layer">
        <svg viewBox="0 0 100 100" preserveAspectRatio="none">
          <circle cx="20" cy="20" r="1" fill="white" />
          <circle cx="50" cy="40" r="1.5" fill="white" />
          <circle cx="80" cy="70" r="1" fill="white" />
          <circle cx="70" cy="20" r="1" fill="white" />
          <circle cx="30" cy="80" r="1.2" fill="white" />
        </svg>
      </div>
      <h1 className="brand">INSPIRI</h1>
      <div className="auth-card">
        <div className="text-center mb-8">
          <p className="subtitle">
            {isSignIn ? "Welcome back" : "Create your account"}
          </p>
        </div>

        <form onSubmit={handleAuth} className="space-y-6">
          <div>
            <input
              type="email"
              placeholder="Email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="auth-input"
              required
            />
          </div>
          
          <div>
            <input
              type="password"
              placeholder="Password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="auth-input"
              required
            />
          </div>

          {!isSignIn && (
            <div>
              <input
                type="text"
                placeholder="Access Code"
                value={accessCode}
                onChange={(e) => setAccessCode(e.target.value)}
                className="auth-input"
                required
              />
              <p className="hint-text">Enter your invitation access code</p>
            </div>
          )}

          {error && <div className="error-box">{error}</div>}

          <button
            type="submit"
            disabled={isLoading}
            className="submit-btn"
          >
            {isLoading ? "Please wait..." : isSignIn ? "Sign In" : "Create Account"}
          </button>
        </form>

        <div className="relative my-6">
          <div className="absolute inset-0 flex items-center">
            <div className="w-full border-t border-white/10"></div>
          </div>
          <div className="relative flex justify-center text-sm">
            <span className="px-2 bg-black text-gray-400">or</span>
          </div>
        </div>

        <button
          onClick={() => alert('Google Sign-In requires API keys. Please provide your Google OAuth credentials.')}
          className="google-btn"
        >
          <svg className="w-5 h-5" viewBox="0 0 24 24">
            <path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
            <path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
            <path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
            <path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
          </svg>
          Continue with Google
        </button>

        <div className="text-center mt-6">
          <button
            onClick={() => setIsSignIn(!isSignIn)}
            className="toggle-btn"
          >
            {isSignIn ? "Need an account? Sign up" : "Already have an account? Sign in"}
          </button>
        </div>
      </div>
    </div>
    <style>{`
      @import url('https://fonts.googleapis.com/css2?family=Inter:wght@900&family=JetBrains+Mono:wght@800&display=swap');
      body {
        margin: 0;
        font-family: 'Inter', 'JetBrains Mono', monospace;
        color: rgba(255,255,255,0.8);
      }
      .auth-wrapper {
        position: relative;
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        overflow: hidden;
      }
      .gradient-bg {
        position: fixed;
        inset: 0;
        background: linear-gradient(120deg, #ff006e, #8338ec, #3a86ff, #06ffa5, #ffbe0b);
        background-size: 200% 200%;
        animation: gradientMove 30s ease infinite;
        z-index: -2;
      }
      @keyframes gradientMove {
        0% {background-position: 0% 50%;}
        50% {background-position: 100% 50%;}
        100% {background-position: 0% 50%;}
      }
      .dust-layer {
        position: fixed;
        inset: 0;
        pointer-events: none;
        opacity: 0.15;
        animation: dustDrift 40s linear infinite;
        z-index: -1;
      }
      @keyframes dustDrift {
        from {transform: translate3d(0,0,0);}
        to {transform: translate3d(-200px,-200px,0);}
      }
      .brand {
        position: absolute;
        top: 5vh;
        width: 100%;
        text-align: center;
        font-family: 'JetBrains Mono', monospace;
        font-size: 72px;
        font-weight: 800;
        color: #000;
        text-shadow: 0 0 2px #fff;
      }
      .auth-card {
        background: rgba(255,255,255,0.06);
        backdrop-filter: blur(14px);
        border-radius: 32px;
        border: 1px solid rgba(255,255,255,0.2);
        max-width: 420px;
        width: 100%;
        padding: 2rem;
      }
      .subtitle {
        font-weight: 300;
      }
      .auth-input {
        width: 100%;
        padding: 1rem;
        background: transparent;
        border: none;
        border-bottom: 1px solid rgba(255,255,255,0.4);
        color: #fff;
        outline: none;
      }
      .auth-input:focus {
        box-shadow: 0 0 0 2px rgba(255,255,255,0.4);
      }
      .submit-btn, .google-btn {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 0.5rem;
        padding: 1rem 1.5rem;
        border-radius: 9999px;
        font-weight: 600;
        color: #fff;
        background: linear-gradient(to bottom right, #ff006e, #8338ec, #3a86ff, #06ffa5, #ffbe0b);
        background-size: 200% 200%;
        transition: background-position 0.5s;
      }
      .submit-btn:hover, .google-btn:hover {
        background-position: 0% 0%;
        box-shadow: 0 0 20px rgba(255,255,255,0.3);
      }
      .error-box {
        background: rgba(255,0,0,0.2);
        border: 1px solid rgba(255,0,0,0.5);
        border-radius: 0.5rem;
        padding: 0.75rem;
        color: #ffdddd;
        font-size: 0.875rem;
      }
      .hint-text {
        font-size: 0.75rem;
        color: rgba(255,255,255,0.5);
        margin-top: 0.5rem;
      }
      .toggle-btn {
        color: rgba(255,255,255,0.5);
        background: none;
        border: none;
        font-size: 0.875rem;
      }
      .toggle-btn:hover {
        color: #fff;
      }
    `}</style>
  );
}
