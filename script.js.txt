// Elements
const menuBtn = document.getElementById("menuBtn");
const navLinks = document.getElementById("navLinks");
const themeBtn = document.getElementById("themeBtn");
const resumeBtn = document.getElementById("resumeBtn");
const yearEl = document.getElementById("year");
const contactForm = document.getElementById("contactForm");

// -------------------- Theme (Dark/Light) --------------------
const THEME_KEY = "vt_theme";

function setTheme(mode) {
  document.body.classList.toggle("light", mode === "light");
  localStorage.setItem(THEME_KEY, mode);
  // icon
  const icon = document.querySelector("#themeBtn .icon");
  if (icon) icon.textContent = mode === "light" ? "☀" : "☾";
}

const savedTheme = localStorage.getItem(THEME_KEY);
if (savedTheme) {
  setTheme(savedTheme);
} else {
  // default dark
  setTheme("dark");
}

themeBtn?.addEventListener("click", () => {
  const isLight = document.body.classList.contains("light");
  setTheme(isLight ? "dark" : "light");
});

// -------------------- Mobile Menu --------------------
menuBtn?.addEventListener("click", () => {
  const isOpen = navLinks.classList.toggle("open");
  menuBtn.classList.toggle("open", isOpen);
  menuBtn.setAttribute("aria-expanded", String(isOpen));
});

navLinks?.querySelectorAll("a").forEach(a => {
  a.addEventListener("click", () => {
    navLinks.classList.remove("open");
    menuBtn.classList.remove("open");
    menuBtn.setAttribute("aria-expanded", "false");
  });
});

// -------------------- Footer Year --------------------
if (yearEl) yearEl.textContent = new Date().getFullYear();

// -------------------- Resume Button --------------------
resumeBtn?.addEventListener("click", () => {
  // Put your resume file in same folder and name it "resume.pdf"
  window.open("resume.pdf", "_blank");
});

// -------------------- Contact Form (mailto) --------------------
contactForm?.addEventListener("submit", (e) => {
  e.preventDefault();
  const fd = new FormData(contactForm);
  const name = (fd.get("name") || "").toString().trim();
  const email = (fd.get("email") || "").toString().trim();
  const message = (fd.get("message") || "").toString().trim();

  const subject = encodeURIComponent(`Website Contact - ${name || "Visitor"}`);
  const body = encodeURIComponent(
    `Name: ${name}\nEmail: ${email}\n\nMessage:\n${message}\n`
  );

  window.location.href = `mailto:Vikramthakor760065@gmail.com?subject=${subject}&body=${body}`;
});

// -------------------- Active link highlight on scroll --------------------
const sections = ["about","experience","work","skills","education","contact"]
  .map(id => document.getElementById(id))
  .filter(Boolean);

const links = Array.from(navLinks?.querySelectorAll("a") || []);

function setActiveLink() {
  const y = window.scrollY + 110;
  let current = sections[0]?.id;

  for (const s of sections) {
    if (s.offsetTop <= y) current = s.id;
  }

  links.forEach(a => {
    const href = a.getAttribute("href") || "";
    a.classList.toggle("active", href === `#${current}`);
  });
}

window.addEventListener("scroll", setActiveLink, { passive: true });
setActiveLink();

// -------------------- Animate skill bars on view --------------------
const barFills = document.querySelectorAll(".bar i[data-p]");

const io = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (!entry.isIntersecting) return;
    const el = entry.target;
    const p = el.getAttribute("data-p");
    el.style.width = `${p}%`;
    io.unobserve(el);
  });
}, { threshold: 0.35 });

barFills.forEach(el => io.observe(el));
