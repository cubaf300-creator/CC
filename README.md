# CC
بحاول
import React, { useMemo, useState, useEffect } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { LogIn, LogOut, Moon, Sun, Mail, Lock, CheckCircle2, XCircle, ShieldCheck, User, Github, Linkedin, Globe } from "lucide-react";

// --- Utility: fake auth ---
const wait = (ms) => new Promise((res) => setTimeout(res, ms));
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

export default function App() {
  const [dark, setDark] = useState(true);
  const [route, setRoute] = useState("home"); // 'home' | 'login' | 'dashboard'
  const [user, setUser] = useState(null);

  useEffect(() => {
    document.documentElement.classList.toggle("dark", dark);
  }, [dark]);

  return (
    <div className="min-h-screen bg-gradient-to-b from-slate-50 to-white dark:from-slate-900 dark:to-slate-950 text-slate-800 dark:text-slate-100">
      <Navbar dark={dark} setDark={setDark} user={user} onLoginClick={() => setRoute("login")} onLogout={() => { setUser(null); setRoute("home"); }} />

      <main className="mx-auto max-w-6xl px-4 sm:px-6 lg:px-8 pt-24 pb-20">
        <AnimatePresence mode="wait">
          {route === "home" && (
            <motion.section
              key="home"
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: -20 }}
              transition={{ duration: 0.4 }}
              className="grid gap-10"
            >
              <Hero onGetStarted={() => setRoute("login")} />
              <Projects />
              <Contact />
            </motion.section>
          )}

          {route === "login" && (
            <motion.section
              key="login"
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: -20 }}
              transition={{ duration: 0.3 }}
              className="grid place-items-center"
            >
              <LoginCard onCancel={() => setRoute("home")} onSuccess={(profile) => { setUser(profile); setRoute("dashboard"); }} />
            </motion.section>
          )}

          {route === "dashboard" && (
            <motion.section
              key="dashboard"
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: -20 }}
              transition={{ duration: 0.3 }}
            >
              <Dashboard user={user} onBackHome={() => setRoute("home")} />
            </motion.section>
          )}
        </AnimatePresence>
      </main>

      <Footer />
    </div>
  );
}

function Navbar({ dark, setDark, user, onLoginClick, onLogout }) {
  return (
    <header className="fixed top-0 inset-x-0 z-50">
      <div className="mx-auto max-w-6xl px-4 sm:px-6 lg:px-8">
        <div className="mt-4 rounded-2xl bg-white/70 dark:bg-slate-900/60 backdrop-blur shadow-sm border border-slate-200/50 dark:border-slate-700/50">
          <nav className="flex items-center justify-between p-3">
            <a href="#" className="flex items-center gap-2 font-semibold tracking-tight">
              <ShieldCheck className="h-6 w-6" />
              <span>PortfolioX</span>
            </a>
            <div className="hidden sm:flex items-center gap-6 text-sm">
              <a className="hover:opacity-80" href="#projects">المشاريع</a>
              <a className="hover:opacity-80" href="#contact">تواصل</a>
              <button
                onClick={() => setDark(!dark)}
                aria-label="Toggle theme"
                className="rounded-xl border px-3 py-1.5 border-slate-300 dark:border-slate-600 hover:bg-slate-100 dark:hover:bg-slate-800"
              >
                {dark ? <Sun className="h-4 w-4" /> : <Moon className="h-4 w-4" />}
              </button>
              {user ? (
                <button onClick={onLogout} className="inline-flex items-center gap-2 rounded-xl border px-4 py-2 text-sm border-rose-300 dark:border-rose-600 hover:bg-rose-50 dark:hover:bg-rose-900/30">
                  <LogOut className="h-4 w-4" /> خروج
                </button>
              ) : (
                <button onClick={onLoginClick} className="inline-flex items-center gap-2 rounded-xl border px-4 py-2 text-sm border-emerald-300 dark:border-emerald-600 hover:bg-emerald-50 dark:hover:bg-emerald-900/30">
                  <LogIn className="h-4 w-4" /> دخول
                </button>
              )}
            </div>
          </nav>
        </div>
      </div>
    </header>
  );
}

function Hero({ onGetStarted }) {
  return (
    <div className="grid md:grid-cols-2 gap-10 items-center">
      <div>
        <motion.h1 className="text-4xl sm:text-5xl font-extrabold tracking-tight" initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} transition={{ delay: 0.1 }}>
          موقع بورتفوليو أنيق مع شاشة تسجيل دخول
        </motion.h1>
        <motion.p className="mt-4 text-slate-600 dark:text-slate-300" initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} transition={{ delay: 0.2 }}>
          مناسب للعرض في الـPortfolio: تصميم حديث، ثيم داكن/فاتح، وتسجيل دخول وهمي لعرض مهارات الـFrontend وواجهة المستخدم.
        </motion.p>
        <div className="mt-6 flex gap-3">
          <button onClick={onGetStarted} className="rounded-2xl shadow px-5 py-3 border border-emerald-400 dark:border-emerald-700 hover:bg-emerald-50 dark:hover:bg-emerald-900/30">جرّب شاشة الدخول</button>
          <a href="#projects" className="rounded-2xl shadow px-5 py-3 border border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800">شوف المشاريع</a>
        </div>
      </div>
      <motion.div initial={{ opacity: 0, scale: 0.96 }} animate={{ opacity: 1, scale: 1 }} transition={{ delay: 0.15 }} className="relative">
        <div className="aspect-video w-full rounded-2xl border border-slate-200 dark:border-slate-800 bg-gradient-to-tr from-emerald-200 via-white to-emerald-100 dark:from-slate-800 dark:via-slate-900 dark:to-slate-800 p-6">
          <div className="h-full w-full grid place-items-center">
            <div className="text-center">
              <p className="text-sm uppercase tracking-widest text-slate-500">Demo</p>
              <h3 className="text-2xl font-semibold mt-2">واجهة احترافية لعرض مهاراتك</h3>
              <p className="mt-2 text-slate-600 dark:text-slate-300">عناصر متحركة، فورم متحقق، وتجربة مستخدم نظيفة.</p>
            </div>
          </div>
        </div>
      </motion.div>
    </div>
  );
}

function LoginCard({ onCancel, onSuccess }) {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [success, setSuccess] = useState(false);

  const canSubmit = useMemo(() => emailRegex.test(email) && password.length >= 6, [email, password]);

  const submit = async (e) => {
    e.preventDefault();
    setError("");
    if (!canSubmit) {
      setError("من فضلك أدخل بريدًا صحيحًا وكلمة مرور 6 أحرف على الأقل");
      return;
    }
    setLoading(true);
    await wait(900); // simulate API
    setLoading(false);
    setSuccess(true);
    await wait(600);
    onSuccess({ name: email.split("@")[0], email });
  };

  return (
    <div className="w-full max-w-md">
      <div className="rounded-2xl border border-slate-200 dark:border-slate-800 bg-white/70 dark:bg-slate-900/60 backdrop-blur shadow p-6">
        <div className="flex items-center gap-2 mb-4">
          <User className="h-5 w-5" />
          <h2 className="text-xl font-bold">تسجيل الدخول</h2>
        </div>
        <form onSubmit={submit} className="grid gap-4">
          <label className="grid gap-1">
            <span className="text-sm text-slate-600 dark:text-slate-300">البريد الإلكتروني</span>
            <div className={`flex items-center gap-2 rounded-xl border px-3 py-2 ${error && !emailRegex.test(email) ? "border-rose-500" : "border-slate-300 dark:border-slate-700"}`}>
              <Mail className="h-4 w-4" />
              <input
                type="email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                placeholder="example@mail.com"
                className="w-full bg-transparent outline-none"
                required
              />
            </div>
          </label>
          <label className="grid gap-1">
            <span className="text-sm text-slate-600 dark:text-slate-300">كلمة المرور</span>
            <div className={`flex items-center gap-2 rounded-xl border px-3 py-2 ${error && password.length < 6 ? "border-rose-500" : "border-slate-300 dark:border-slate-700"}`}>
              <Lock className="h-4 w-4" />
              <input
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                placeholder="••••••••"
                className="w-full bg-transparent outline-none"
                minLength={6}
                required
              />
            </div>
          </label>
          <AnimatePresence>
            {error && (
              <motion.p initial={{ opacity: 0, y: 6 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0, y: -6 }} className="text-sm text-rose-600 flex items-center gap-2">
                <XCircle className="h-4 w-4" /> {error}
              </motion.p>
            )}
          </AnimatePresence>
          <div className="flex items-center gap-3">
            <button type="submit" disabled={loading} className="flex-1 rounded-xl border px-4 py-2 border-emerald-400 dark:border-emerald-700 hover:bg-emerald-50 dark:hover:bg-emerald-900/30 disabled:opacity-60">
              {loading ? "جارٍ التحقق…" : "دخول"}
            </button>
            <button type="button" onClick={onCancel} className="rounded-xl border px-4 py-2 border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800">إلغاء</button>
          </div>
          <AnimatePresence>
            {success && (
              <motion.p initial={{ opacity: 0, y: 6 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0, y: -6 }} className="text-sm text-emerald-600 flex items-center gap-2">
                <CheckCircle2 className="h-4 w-4" /> تم تسجيل الدخول بنجاح
              </motion.p>
            )}
          </AnimatePresence>
        </form>
        <p className="mt-4 text-xs text-slate-500">* هذا نظام دخول تجريبي (بدون باك إند) لعرض مهارات الواجهة فقط.</p>
      </div>
    </div>
  );
}

function Projects() {
  const items = [
    { title: "لوحة تحكم تفاعلية", desc: "Charts + Filters + Dark Mode", link: "#" },
    { title: "Landing Page مودرن", desc: "Hero + CTA + Pricing", link: "#" },
    { title: "تطبيق مهام", desc: "CRUD + LocalStorage", link: "#" },
  ];
  return (
    <section id="projects" className="mt-16">
      <h2 className="text-2xl font-bold mb-6">نماذج مشاريع</h2>
      <div className="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
        {items.map((it, i) => (
          <motion.a key={i} href={it.link} whileHover={{ y: -4 }} className="block rounded-2xl border border-slate-200 dark:border-slate-800 p-5 bg-white/60 dark:bg-slate-900/50 shadow-sm">
            <div className="h-28 rounded-xl bg-gradient-to-tr from-slate-200 to-white dark:from-slate-800 dark:to-slate-900 mb-4" />
            <h3 className="font-semibold">{it.title}</h3>
            <p className="text-sm text-slate-600 dark:text-slate-300">{it.desc}</p>
          </motion.a>
        ))}
      </div>
    </section>
  );
}

function Dashboard({ user, onBackHome }) {
  return (
    <section className="grid gap-6">
      <div className="rounded-2xl border border-slate-200 dark:border-slate-800 bg-white/70 dark:bg-slate-900/60 backdrop-blur p-6">
        <div className="flex items-center gap-3">
          <ShieldCheck className="h-6 w-6" />
          <h2 className="text-xl font-bold">أهلًا {user?.name || "مستخدم"} 👋</h2>
        </div>
        <p className="mt-2 text-slate-600 dark:text-slate-300">دي صفحة محمية تظهر بعد تسجيل الدخول. تقدر تعرض فيها إحصائيات أو عناصر تخص مشروعك.</p>
        <div className="mt-4 grid sm:grid-cols-3 gap-4">
          {["جلسات اليوم", "معدّل التحويل", "أخطاء الواجهات"].map((k, i) => (
            <div key={i} className="rounded-xl border border-slate-200 dark:border-slate-800 p-4">
              <p className="text-sm text-slate-500">{k}</p>
              <p className="text-2xl font-semibold">{[128, "3.1%", 0][i]}</p>
            </div>
          ))}
        </div>
        <button onClick={onBackHome} className="mt-6 rounded-xl border px-4 py-2 border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800">الرجوع للواجهة</button>
      </div>
    </section>
  );
}

function Contact() {
  return (
    <section id="contact" className="mt-16">
      <h2 className="text-2xl font-bold mb-6">تواصل معي</h2>
      <div className="rounded-2xl border border-slate-200 dark:border-slate-800 bg-white/70 dark:bg-slate-900/60 backdrop-blur p-6">
        <div className="flex flex-wrap items-center gap-3">
          <a className="inline-flex items-center gap-2 rounded-xl border px-4 py-2 border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800" href="#">
            <Github className="h-4 w-4" /> GitHub
          </a>
          <a className="inline-flex items-center gap-2 rounded-xl border px-4 py-2 border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800" href="#">
            <Linkedin className="h-4 w-4" /> LinkedIn
          </a>
          <a className="inline-flex items-center gap-2 rounded-xl border px-4 py-2 border-slate-300 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-800" href="#">
            <Globe className="h-4 w-4" /> Website
          </a>
        </div>
        <p className="mt-3 text-sm text-slate-600 dark:text-slate-300">بدّل الروابط دي بروابطك الحقيقية قبل ما ترفعه على GitHub Pages أو Netlify.</p>
      </div>
    </section>
  );
}

function Footer() {
  return (
    <footer className="py-10">
      <div className="mx-auto max-w-6xl px-4 sm:px-6 lg:px-8">
        <div className="rounded-2xl border border-slate-200 dark:border-slate-800 bg-white/60 dark:bg-slate-900/50 p-6 text-sm flex flex-col sm:flex-row items-center justify-between gap-3">
          <p>© {new Date().getFullYear()} PortfolioX. All rights reserved.</p>
          <p className="text-slate-500">Built with React + Tailwind + Framer Motion</p>
        </div>
      </div>
    </footer>
  );
}
