import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import { createPageUrl } from "./utils";
import { motion, AnimatePresence } from "framer-motion";
import { Menu, X, ArrowUp, Mail, Linkedin, Instagram } from "lucide-react";

export default function Layout({ children, currentPageName }) {
  const [isScrolled, setIsScrolled] = useState(false);
  const [isMobileMenuOpen, setIsMobileMenuOpen] = useState(false);
  const [showScrollTop, setShowScrollTop] = useState(false);

  useEffect(() => {
    const handleScroll = () => {
      setIsScrolled(window.scrollY > 50);
      setShowScrollTop(window.scrollY > 500);
    };
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  const navItems = [
    { name: "Accueil", page: "Home" },
    { name: "À propos", page: "About" },
    { name: "Formations", page: "Formations" },
    { name: "Services", page: "Services" },
    { name: "Témoignages", page: "Testimonials" },
    { name: "Contact", page: "Contact" },
  ];

  const scrollToTop = () => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  };

  return (
    <div className="min-h-screen bg-white">
      <style>{`
        :root {
          --color-primary: #0A0A0A;
          --color-secondary: #1A1A1A;
          --color-accent: #C9A050;
          --color-accent-light: #E5C882;
          --color-text: #333333;
          --color-text-light: #666666;
          --color-bg: #FFFFFF;
          --color-bg-cream: #FDFBF7;
          --color-bg-dark: #0A0A0A;
        }
        
        @keyframes shimmer {
          0% { background-position: -200% 0; }
          100% { background-position: 200% 0; }
        }
        
        .text-gradient {
          background: linear-gradient(135deg, #C9A050 0%, #E5C882 50%, #C9A050 100%);
          -webkit-background-clip: text;
          -webkit-text-fill-color: transparent;
          background-clip: text;
        }
        
        .gold-border {
          border-image: linear-gradient(135deg, #C9A050, #E5C882, #C9A050) 1;
        }
        
        .premium-shadow {
          box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.08), 0 12px 24px -8px rgba(201, 160, 80, 0.05);
        }
        
        html {
          scroll-behavior: smooth;
        }
      `}</style>

      {/* Navigation */}
      <motion.header
        initial={{ y: -100 }}
        animate={{ y: 0 }}
        transition={{ duration: 0.6, ease: "easeOut" }}
        className={`fixed top-0 left-0 right-0 z-50 transition-all duration-500 ${
          isScrolled
            ? "bg-white/95 backdrop-blur-md shadow-lg py-4"
            : "bg-transparent py-6"
        }`}
      >
        <div className="max-w-7xl mx-auto px-6 lg:px-12">
          <div className="flex items-center justify-between">
            <Link
              to={createPageUrl("Home")}
              className="relative group"
            >
              <span className={`text-2xl font-light tracking-[0.2em] uppercase transition-colors duration-300 ${
                isScrolled ? "text-[#0A0A0A]" : "text-white"
              }`}>
                My Design
              </span>
              <span className="block h-[1px] w-0 group-hover:w-full bg-[#C9A050] transition-all duration-500" />
            </Link>

            {/* Desktop Navigation */}
            <nav className="hidden lg:flex items-center gap-10">
              {navItems.map((item) => (
                <Link
                  key={item.page}
                  to={createPageUrl(item.page)}
                  className={`relative text-sm font-medium tracking-wide transition-colors duration-300 group ${
                    currentPageName === item.page
                      ? "text-[#C9A050]"
                      : isScrolled
                      ? "text-[#333333] hover:text-[#C9A050]"
                      : "text-white/90 hover:text-white"
                  }`}
                >
                  {item.name}
                  <span className={`absolute -bottom-1 left-0 h-[1px] bg-[#C9A050] transition-all duration-300 ${
                    currentPageName === item.page ? "w-full" : "w-0 group-hover:w-full"
                  }`} />
                </Link>
              ))}
            </nav>

            {/* CTA Button */}
            <Link
              to={createPageUrl("Contact")}
              className={`hidden lg:flex items-center gap-2 px-6 py-3 border transition-all duration-300 text-sm font-medium tracking-wide ${
                isScrolled
                  ? "border-[#0A0A0A] text-[#0A0A0A] hover:bg-[#0A0A0A] hover:text-white"
                  : "border-white/50 text-white hover:bg-white hover:text-[#0A0A0A]"
              }`}
            >
              <Mail className="w-4 h-4" />
              Prendre contact
            </Link>

            {/* Mobile Menu Button */}
            <button
              onClick={() => setIsMobileMenuOpen(!isMobileMenuOpen)}
              className={`lg:hidden p-2 transition-colors ${
                isScrolled ? "text-[#0A0A0A]" : "text-white"
              }`}
            >
              {isMobileMenuOpen ? <X className="w-6 h-6" /> : <Menu className="w-6 h-6" />}
            </button>
          </div>
        </div>

        {/* Mobile Menu */}
        <AnimatePresence>
          {isMobileMenuOpen && (
            <motion.div
              initial={{ opacity: 0, height: 0 }}
              animate={{ opacity: 1, height: "auto" }}
              exit={{ opacity: 0, height: 0 }}
              className="lg:hidden bg-white border-t"
            >
              <nav className="max-w-7xl mx-auto px-6 py-6 flex flex-col gap-4">
                {navItems.map((item) => (
                  <Link
                    key={item.page}
                    to={createPageUrl(item.page)}
                    onClick={() => setIsMobileMenuOpen(false)}
                    className={`text-lg font-medium py-2 transition-colors ${
                      currentPageName === item.page
                        ? "text-[#C9A050]"
                        : "text-[#333333] hover:text-[#C9A050]"
                    }`}
                  >
                    {item.name}
                  </Link>
                ))}
                <Link
                  to={createPageUrl("Contact")}
                  onClick={() => setIsMobileMenuOpen(false)}
                  className="mt-4 py-4 bg-[#0A0A0A] text-white text-center font-medium"
                >
                  Prendre contact
                </Link>
              </nav>
            </motion.div>
          )}
        </AnimatePresence>
      </motion.header>

      {/* Main Content */}
      <main>{children}</main>

      {/* Footer */}
      <footer className="bg-[#0A0A0A] text-white">
        <div className="max-w-7xl mx-auto px-6 lg:px-12 py-20">
          <div className="grid md:grid-cols-4 gap-12 lg:gap-20">
            <div className="md:col-span-2">
              <span className="text-2xl font-light tracking-[0.2em] uppercase">
                My Design
              </span>
              <p className="mt-6 text-white/60 leading-relaxed max-w-md">
                Formations et services premium pour accompagner votre transformation 
                et atteindre l'excellence dans votre domaine.
              </p>
              <div className="flex items-center gap-4 mt-8">
                <a href="#" className="w-10 h-10 border border-white/20 flex items-center justify-center hover:border-[#C9A050] hover:text-[#C9A050] transition-colors">
                  <Linkedin className="w-4 h-4" />
                </a>
                <a href="#" className="w-10 h-10 border border-white/20 flex items-center justify-center hover:border-[#C9A050] hover:text-[#C9A050] transition-colors">
                  <Instagram className="w-4 h-4" />
                </a>
                <a href="#" className="w-10 h-10 border border-white/20 flex items-center justify-center hover:border-[#C9A050] hover:text-[#C9A050] transition-colors">
                  <Mail className="w-4 h-4" />
                </a>
              </div>
            </div>

            <div>
              <h4 className="text-sm font-semibold tracking-wider uppercase text-[#C9A050] mb-6">
                Navigation
              </h4>
              <nav className="flex flex-col gap-3">
                {navItems.map((item) => (
                  <Link
                    key={item.page}
                    to={createPageUrl(item.page)}
                    className="text-white/60 hover:text-white transition-colors"
                  >
                    {item.name}
                  </Link>
                ))}
              </nav>
            </div>

            <div>
              <h4 className="text-sm font-semibold tracking-wider uppercase text-[#C9A050] mb-6">
                Contact
              </h4>
              <div className="flex flex-col gap-3 text-white/60">
                <p>gbamakossiemmanuel1@gmail.com</p>
                <p>+228 99924713</p>
                <p className="mt-4">Togo</p>
              </div>
            </div>
          </div>

          <div className="mt-16 pt-8 border-t border-white/10 flex flex-col md:flex-row justify-between items-center gap-4">
            <p className="text-white/40 text-sm">
              © {new Date().getFullYear()} My Design. Tous droits réservés.
            </p>
            <div className="flex items-center gap-6 text-sm text-white/40">
              <a href="#" className="hover:text-white transition-colors">Mentions légales</a>
              <a href="#" className="hover:text-white transition-colors">Politique de confidentialité</a>
            </div>
          </div>
        </div>
      </footer>

      {/* Scroll to Top Button */}
      <AnimatePresence>
        {showScrollTop && (
          <motion.button
            initial={{ opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            exit={{ opacity: 0, scale: 0.8 }}
            onClick={scrollToTop}
            className="fixed bottom-8 right-8 w-12 h-12 bg-[#C9A050] text-white flex items-center justify-center shadow-lg hover:bg-[#B8923F] transition-colors z-40"
          >
            <ArrowUp className="w-5 h-5" />
          </motion.button>
        )}
      </AnimatePresence>

      {/* WhatsApp Button */}
      <motion.a
        href="https://wa.me/22899924713"
        target="_blank"
        rel="noopener noreferrer"
        initial={{ opacity: 0, scale: 0.8 }}
        animate={{ opacity: 1, scale: 1 }}
        transition={{ delay: 0.5 }}
        className="fixed bottom-8 left-8 w-14 h-14 bg-[#25D366] text-white flex items-center justify-center rounded-full shadow-lg hover:bg-[#20BA5A] transition-all hover:scale-110 z-40 group"
        title="Contactez-nous sur WhatsApp"
      >
        <svg
          className="w-7 h-7"
          fill="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"/>
        </svg>
        <span className="absolute -top-1 -right-1 w-3 h-3 bg-red-500 rounded-full animate-pulse" />
      </motion.a>
    </div>
  );
}
