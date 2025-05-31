import React, { useState } from "react";
import { motion } from "framer-motion";
import { Button } from "@/components/ui/button";

function ScanIndicator({ scanning }) {
  return scanning ? (
    <motion.div
      className="w-24 h-24 border-8 border-blue-600 rounded-full animate-spin"
      initial={{ scale: 0 }}
      animate={{ scale: 1 }}
      aria-label="Escaneando"
      role="img"
    />
  ) : (
    <div className="text-2xl" aria-hidden="true">
      👆
    </div>
  );
}

function ForceButtons({ setForceResult, forceResult }) {
  return (
    <div className="mt-6 space-y-2">
      <p className="text-sm text-gray-600">Modo broma:</p>
      <div className="flex justify-center gap-2">
        <Button
          onClick={() => setForceResult("VERDAD")}
          className={`bg-green-500 hover:bg-green-600 ${forceResult === "VERDAD" ? "ring-2 ring-green-400" : ""}`}
          aria-pressed={forceResult === "VERDAD"}
        >
          Forzar Verdad
        </Button>
        <Button
          onClick={() => setForceResult("MENTIRA")}
          className={`bg-red-500 hover:bg-red-600 ${forceResult === "MENTIRA" ? "ring-2 ring-red-400" : ""}`}
          aria-pressed={forceResult === "MENTIRA"}
        >
          Forzar Mentira
        </Button>
        <Button
          onClick={() => setForceResult(null)}
          className={`bg-gray-500 hover:bg-gray-600 ${forceResult === null ? "ring-2 ring-gray-400" : ""}`}
          aria-pressed={forceResult === null}
        >
          Aleatorio
        </Button>
      </div>
    </div>
  );
}

export default function LieDetectorApp() {
  const [status, setStatus] = useState(null);
  const [scanning, setScanning] = useState(false);
  const [forceResult, setForceResult] = useState(null);

  const startScan = () => {
    if (scanning) return; // seguridad extra por si acaso
    setStatus(null);
    setScanning(true);

    // Si está en modo aleatorio, reseteamos forceResult para que no se quede pillado en uno
    if (forceResult === null) {
      // nada, dejamos forceResult como está para usar aleatorio
    }

    // Vibrar un poco si se puede (modo hype)
    if (navigator.vibrate) navigator.vibrate(200);

    setTimeout(() => {
      const result = forceResult || (Math.random() > 0.5 ? "VERDAD" : "MENTIRA");
      setStatus(result);
      setScanning(false);
    }, 3000);
  };

  return (
    <main className="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-black to-gray-800 text-white p-6 space-y-8">
      <motion.h1
        initial={{ y: -50, opacity: 0 }}
        animate={{ y: 0, opacity: 1 }}
        className="text-4xl font-bold"
      >
        🔍 Detector de Mentiras
      </motion.h1>

      <div className="w-full max-w-sm bg-white text-black rounded-2xl shadow-2xl p-6 text-center">
        <p className="mb-4 text-lg">Pon el dedo en la pantalla y espera...</p>

        <div
          className="relative w-full h-40 flex items-center justify-center bg-gray-200 rounded-xl"
          role="region"
          aria-live="polite"
          aria-label={scanning ? "Escaneando" : "Esperando escaneo"}
        >
          <ScanIndicator scanning={scanning} />
        </div>

        <Button
          onClick={startScan}
          className="mt-4 w-full bg-red-600 hover:bg-red-700 text-white font-bold text-lg py-2 rounded-xl disabled:opacity-70 disabled:cursor-not-allowed"
          disabled={scanning}
          aria-disabled={scanning}
          aria-busy={scanning}
        >
          {scanning ? "Escaneando..." : "¡Escanear!"}
        </Button>

        {status && (
          <motion.p
            initial={{ scale: 0 }}
            animate={{ scale: 1 }}
            className={`mt-6 text-3xl font-extrabold ${
              status === "VERDAD" ? "text-green-600" : "text-red-600"
            }`}
            aria-live="polite"
          >
            {status === "VERDAD" ? "✅ VERDAD" : "❌ MENTIRA"}
          </motion.p>
        )}

        <ForceButtons setForceResult={setForceResult} forceResult={forceResult} />
      </div>
    </main>
  );
}
