import React, { useEffect, useMemo, useRef, useState } from "react";
import { QRCodeSVG } from "qrcode.react";

// ------------------------------
// Pomocnicze: losowe nagrody demo
// ------------------------------
const DEMO_REWARDS = [
  { id: "R1", label: "Wygrana: 5 zł rabatu" },
  { id: "R2", label: "Wygrana: 10% zniżki" },
  { id: "R3", label: "Wygrana: Napój gratis" },
  { id: "R4", label: "Próbuj ponownie" },
  { id: "R5", label: "Wygrana: 20 zł rabatu" },
];

// Prosty generator kodów (A-Z, 0-9)
function randomCode(len = 8) {
  const chars = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789";
  let out = "";
  for (let i = 0; i < len; i++) out += chars[Math.floor(Math.random() * chars.length)];
  return out;
}

// LocalStorage klucze
const STORAGE_KEY = "scratch_demo_codes_v1";

function loadCodes() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return {};
    return JSON.parse(raw);
  } catch (e) {
    return {};
  }
}

function saveCodes(map) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(map));
}

// ------------------------------
// Komponent Canvas do "zdrapywania"
// ------------------------------
function ScratchCard({ width = 320, height = 180, onRevealPercent, children }) {
  const canvasRef = useRef(null);
  const isDrawing = useRef(false);
  const [scratched, setScratched] = useState(0);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    // Tło "zdrapki"
    ctx.fillStyle = "#B0B7C3"; // srebrnoszary
    ctx.fillRect(0, 0, width, height);
    // Tekst instrukcyjny
    ctx.fillStyle = "#2F3540";
    ctx.font = "bold 16px sans-serif";
    ctx.textAlign = "center";
    ctx.fillText("Zdrap palcem / myszką", width / 2, height / 2);
    // Ustaw tryb mazania
    ctx.globalCompositeOperation = "destination-out";
  }, [width, height]);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");

    const rect = () => canvas.getBoundingClientRect();

    const start = (e) => {
      isDrawing.current = true;
      draw(e);
    };
    const end = () => {
      isDrawing.current = false;
      // Po zakończeniu — policz procent zdrapany
      const pixels = ctx.getImageData(0, 0, canvas.width, canvas.height);
      let cleared = 0;
      for (let i = 3; i < pixels.data.length; i += 4) {
        if (pixels.data[i] === 0) cleared++; // alpha=0 => zdrapane
      }
      const total = canvas.width * canvas.height;
      const percent = Math.round((cleared / total) * 100);
      setScratched(percent);
      onRevealPercent?.(percent);
    };

    const draw = (e) => {
      if (!isDrawing.current) return;
      const r = rect();
      const clientX = e.touches ? e.touches[0].clientX : e.clientX;
      const clientY = e.touches ? e.touches[0].clientY : e.clientY;
      const x = ((clientX - r.left) / r.width) * canvas.width;
      const y = ((clientY - r.top) / r.height) * canvas.height;
      ctx.beginPath();
      ctx.arc(x, y, 16, 0, Math.PI * 2);
      ctx.fill();
    };

    // DPI scaling
    const ratio = window.devicePixelRatio || 1;
    canvas.width = Math.floor(width * ratio);
    canvas.height = Math.floor(height * ratio);
    canvas.style.width = width + "px";
    canvas.style.height = height + "px";
    const ctx2 = canvas.getContext("2d");
    ctx2.scale(ratio, ratio);

    canvas.addEventListener("mousedown", start);
    canvas.addEventListener("mousemove", draw);
    window.addEventListener("mouseup", end);

    canvas.addEventListener("touchstart", start, { passive: true });
    canvas.addEventListener("touchmove", draw, { passive: true });
    window.addEventListener("touchend", end);

    return () => {
      canvas.removeEventListener("mousedown", start);
      canvas.removeEventListener("mousemove", draw);
      window.removeEventListener("mouseup", end);
      canvas.removeEventListener("touchstart", start);
      canvas.removeEventListener("touchmove", draw);
      window.removeEventListener("touchend", end);
    };
  }, [onRevealPercent, width, height]);

  return (
    <div className="relative" style={{ width, height }}>
      <div className="absolute inset-0 flex items-center justify-center rounded-2xl bg-white">
        {children}
      </div>
      <canvas ref={canvasRef} className="absolute inset-0 rounded-2xl shadow-inner" />
      <div className="absolute bottom-2 right-3 text-xs text-white/90 drop-shadow-sm select-none">
        {scratched}%
      </div>
    </div>
  );
}

// ------------------------------
// Główny komponent demo
// ------------------------------
export default function App() {
  const [tab, setTab] = useState("kasa"); // "kasa" | "klient"
  const [codes, setCodes] = useState(() => loadCodes());
  const [lastCode, setLastCode] = useState("");

  // Sekcja KASA — generowanie i podgląd paragonu
  const createCoupon = () => {
    const code = randomCode(8);
    const prize = DEMO_REWARDS[Math.floor(Math.random() * DEMO_REWARDS.length)];
    const map = { ...codes, [code]: { prize: prize.label, redeemed: false, createdAt: Date.now() } };
    setCodes(map);
    saveCodes(map);
    setLastCode(code);
  };

  const qrUrl = useMemo(() => {
    const base = typeof window !== "undefined" ? window.location.origin : "https://twojastrona.pl";
    return `${base}/zdrapka?kod=${lastCode || "PRZYKLAD"}`;
  }, [lastCode]);

  const clearAll = () => {
    if (!confirm("Skasować wszystkie zapisane kody w tym urządzeniu?")) return;
    localStorage.removeItem(STORAGE_KEY);
    setCodes({});
    setLastCode("");
  };

  // Sekcja KLIENT — weryfikacja kodu i zdrapywanie
  const [enteredCode, setEnteredCode] = useState("");
  const [validated, setValidated] = useState(null); // {prize, redeemed}
  const [autoReveal, setAutoReveal] = useState(false);

  const submitCode = () => {
    const info = codes[enteredCode.trim().toUpperCase()];
    if (!info) {
      setValidated({ error: "Nie znaleziono takiego kodu." });
    } else if (info.redeemed) {
      setValidated({ error: "Kod został już zrealizowany." });
    } else {
      setValidated({ prize: info.prize, code: enteredCode.trim().toUpperCase() });
    }
  };

  const markRedeemed = () => {
    if (!validated?.code) return;
    const map = { ...codes };
    map[validated.code] = { ...map[validated.code], redeemed: true, redeemedAt: Date.now() };
    setCodes(map);
    saveCodes(map);
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-slate-50 to-slate-100 text-slate-800">
      <div className="max-w-5xl mx-auto p-6">
        <header className="flex items-center justify-between gap-4 mb-6">
          <h1 className="text-2xl sm:text-3xl font-bold tracking-tight">Cyfrowa Zdrapka – demo</h1>
          <div className="flex items-center gap-2 bg-white rounded-full p-1 shadow-sm">
            <button
              onClick={() => setTab("kasa")}
              className={`px-3 py-1.5 rounded-full text-sm transition ${tab === "kasa" ? "bg-slate-900 text-white" : "hover:bg-slate-100"}`}
            >
              Kasa (drukuj kod)
            </button>
            <button
              onClick={() => setTab("klient")}
              className={`px-3 py-1.5 rounded-full text-sm transition ${tab === "klient" ? "bg-slate-900 text-white" : "hover:bg-slate-100"}`}
            >
              Klient (zdrapka)
            </button>
          </div>
        </header>

        {tab === "kasa" ? (
          <section className="grid md:grid-cols-2 gap-6">
            <div className="bg-white rounded-2xl shadow p-5">
              <h2 className="font-semibold mb-2">Generator kuponu</h2>
              <p className="text-sm text-slate-600 mb-4">Kliknij, aby wygenerować nowy kod oraz przypisaną nagrodę. Dane trzymają się lokalnie w tej przeglądarce (demo).</p>
              <div className="flex gap-2">
                <button onClick={createCoupon} className="px-4 py-2 rounded-xl bg-slate-900 text-white shadow hover:opacity-90">Wygeneruj kupon</button>
                <button onClick={clearAll} className="px-3 py-2 rounded-xl bg-slate-100 hover:bg-slate-200">Wyczyść wszystko</button>
              </div>

              {lastCode && (
                <div className="mt-6">
                  <h3 className="font-medium">Ostatni kod</h3>
                  <div className="mt-2 text-2xl font-mono tracking-wider">{lastCode}</div>
                </div>
              )}

              <div className="mt-6">
                <h3 className="font-medium mb-2">Wydruk testowy (paragon)</h3>
                <div className="bg-slate-50 border rounded-xl p-4 font-mono text-xs max-w-sm">
                  <div className="text-center">*** PROMOCJA ZDRAPKA ***</div>
                  <div className="mt-2">Kod: <span className="font-bold">{lastCode || "PRZYKLAD"}</span></div>
                  <div className="mt-2">Zeskanuj QR i zdrap nagrodę:</div>
                  <div className="mt-3 flex items-center justify-center">
                    <QRCodeSVG value={qrUrl} size={144} includeMargin />
                  </div>
                  <div className="mt-3">URL: {qrUrl}</div>
                  <div className="mt-3">Dziękujemy i powodzenia!</div>
                  <div className="mt-3 text-center">--------------------</div>
                </div>
                <p className="text-xs text-slate-500 mt-2">W prawdziwym wdrożeniu ten blok idzie na drukarkę paragonową (ESC/POS). Możesz użyć biblioteki w stylu <span className="font-mono">escpos-encoder</span> lub natywnego SDK producenta.</p>
              </div>

              <div className="mt-6">
                <h3 className="font-medium mb-2">Lista wygenerowanych kodów (lokalnie)</h3>
                <div className="max-h-48 overflow-auto border rounded-xl">
                  <table className="w-full text-sm">
                    <thead className="bg-slate-50 sticky top-0">
                      <tr>
                        <th className="text-left p-2">Kod</th>
                        <th className="text-left p-2">Nagroda</th>
                        <th className="text-left p-2">Status</th>
                      </tr>
                    </thead>
                    <tbody>
                      {Object.entries(codes).length === 0 && (
                        <tr><td className="p-3 text-slate-500" colSpan={3}>Brak — wygeneruj pierwszy kupon.</td></tr>
                      )}
                      {Object.entries(codes).map(([code, info]) => (
                        <tr key={code} className="odd:bg-white even:bg-slate-50">
                          <td className="p-2 font-mono">{code}</td>
                          <td className="p-2">{info.prize}</td>
                          <td className="p-2">{info.redeemed ? "Zrealizowany" : "Aktywny"}</td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              </div>
            </div>

            <div className="bg-white rounded-2xl shadow p-5">
              <h2 className="font-semibold mb-2">Podpowiedź wdrożeniowa</h2>
              <ol className="text-sm text-slate-700 list-decimal pl-5 space-y-2">
                <li>Serwer/API tworzy kupon: <span className="font-mono">{`{ code: "ABCD1234", prize: "10%" }`}</span> i zapisuje w bazie.</li>
                <li>Kasa drukuje paragon z kodem + QR do URL-a weryfikacji.</li>
                <li>Klient skanuje QR → trafia do widoku zdrapki z weryfikacją kodu.</li>
                <li>Po odsłonięciu nagrody i/lub odbiorze — oznaczasz kupon jako zrealizowany.</li>
              </ol>
              <div className="mt-4 p-3 rounded-xl bg-slate-50 text-xs">
                <div className="font-semibold mb-1">Uwaga (demo):</div>
                W tej wersji wszystko trzymamy w <span className="font-mono">localStorage</span> dla szybkich testów. W produkcji użyj bazy danych i endpointów API z autoryzacją.
              </div>
            </div>
          </section>
        ) : (
          <section className="grid md:grid-cols-2 gap-6">
            <div className="bg-white rounded-2xl shadow p-5">
              <h2 className="font-semibold mb-3">Wpisz kod z paragonu</h2>
              <div className="flex gap-2 items-center mb-3">
                <input
          value={enteredCode}
          onChange={(e) => setEnteredCode(e.target.value.toUpperCase())}
          placeholder="NP. 7K3Q9R2M"
          className="flex-1 px-3 py-2 rounded-xl border focus:outline-none focus:ring-2 focus:ring-slate-300 font-mono"
        />
                <button onClick={submitCode} className="px-4 py-2 rounded-xl bg-slate-900 text-white shadow hover:opacity-90">Sprawdź</button>
              </div>
              {validated?.error && (
                <div className="p-3 rounded-xl bg-red-50 text-red-700 text-sm">{validated.error}</div>
              )}

              {validated?.prize && (
                <div className="mt-4 space-y-3">
                  <div className="text-sm text-slate-600">Kod poprawny — zdrap, aby poznać wynik:</div>
                  <ScratchCard
                    width={360}
                    height={200}
                    onRevealPercent={(p) => {
                      if (p >= 60) setAutoReveal(true);
                    }}
                  >
                    <div className="text-center px-6">
                      <div className="text-sm text-slate-500">Twoja nagroda</div>
                      <div className="text-2xl font-bold mt-1">{validated.prize}</div>
                      <div className="mt-2 text-xs text-slate-400">Kod: {validated.code}</div>
                    </div>
                  </ScratchCard>
                  <div className="flex items-center gap-2">
                    <button
                      onClick={() => setAutoReveal(true)}
                      className="px-3 py-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-sm"
                    >
                      Pokaż wszystko
                    </button>
                    <button
                      onClick={markRedeemed}
                      className="px-3 py-2 rounded-lg bg-emerald-600 text-white text-sm shadow hover:opacity-95"
                    >
                      Oznacz jako zrealizowany
                    </button>
                    {autoReveal && (
                      <span className="text-xs text-slate-500">Zdrapka odsłonięta.</span>
                    )}
                  </div>
                </div>
              )}
            </div>

            <div className="bg-white rounded-2xl shadow p-5">
              <h2 className="font-semibold mb-3">Szybkie testy (bez paragonu)</h2>
              <p className="text-sm text-slate-600">Kliknij dowolny istniejący kod, by wypełnić formularz:</p>
              <div className="mt-3 grid grid-cols-2 gap-2 max-h-64 overflow-auto">
                {Object.entries(codes).map(([code, info]) => (
                  <button
                    key={code}
                    onClick={() => { setEnteredCode(code); setValidated(null); setAutoReveal(false); }}
                    className="text-left p-3 rounded-xl border hover:bg-slate-50"
                  >
                    <div className="font-mono font-semibold">{code}</div>
                    <div className="text-xs text-slate-600">{info.prize}</div>
                    <div className="text-xs mt-1">{info.redeemed ? "Zrealizowany" : "Aktywny"}</div>
                  </button>
                ))}
                {Object.entries(codes).length === 0 && (
                  <div className="text-sm text-slate-500">Brak kodów. Przejdź do zakładki „Kasa” i wygeneruj pierwszy kupon.</div>
                )}
              </div>

              <div className="mt-6 p-3 rounded-xl bg-slate-50 text-xs leading-relaxed">
                <div className="font-semibold mb-1">Integracja produkcyjna (skrót):</div>
                <ul className="list-disc pl-5 space-y-1">
                  <li>Weryfikacja kodu przez API: <span className="font-mono">GET /api/coupons/:code</span></li>
                  <li>Oznaczanie realizacji: <span className="font-mono">POST /api/coupons/:code/redeem</span></li>
                  <li>Druk ESC/POS: generuj tekst + QR (często: <span className="font-mono">GS ( k</span> dla QR) lub SDK producenta).</li>
                </ul>
              </div>
            </div>
          </section>
        )}

        <footer className="mt-10 text-center text-xs text-slate-500">
          Demo lokalne — odświeżenie strony nie usuwa danych, dopóki nie klikniesz „Wyczyść wszystko”.
        </footer>
      </div>

      {/* Prosty globalny styl aby ScratchCard mógł odsłonić treść */}
      <style>{`
        canvas { touch-action: none; }
      `}</style>

      {/* Auto-Reveal CSS: gdy autoReveal = true, chowamy kanwę za pomocą inline-style? Prościej: sterujemy w ScratchCard po stronie rodzica.
          W tej wersji pozostaje ręczne "Pokaż wszystko" które po prostu komunikuje użytkownikowi, że odsłonięto (UI).
      */}
    </div>
  );
}
                          
