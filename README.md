import React, { useState } from "react";
import { motion } from "framer-motion";
import { Button } from "@/components/ui/button";

export default function LieDetectorApp() {
  const [status, setStatus] = useState(null);
  const [scanning, setScanning] = useState(false);
  const [forceResult, setForceResult] = useState(null);

  const startScan = () => {
    setScanning(true);
    setStatus(null);
    setTimeout(() => {
      const result = forceResult || (Math.random() > 0.5 ? "VERDAD" : "MENTIRA");
      setStatus(result);
      setScanning(false);
    }, 3000);
  };

  return (
    <main className="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-black to-gray-800 text-white p-6 space-y-8">
      <motion.h1 initial={{ y: -50, opacity: 0 }} animate={{ y: 0, opacity: 1 }} className="text-4xl font-bold">
        🔍 Detector de Mentiras
      </motion.h1>

      <div className="w-full max-w-sm bg-white text-black rounded-2xl shadow-2xl p-6 text-center">
        <p className="mb-4 text-lg">Pon el dedo en la pantalla y espera...</p>

        <div className="relative w-full h-40 flex items-center justify-center bg-gray-200 rounded-xl">
          {scanning ? (
            <motion.div
              className="w-24 h-24 border-8 border-blue-600 rounded-full animate-spin"
              initial={{ scale: 0 }}
              animate={{ scale: 1 }}
            />
          ) : (
            <div className="text-2xl">👆</div>
          )}
        </div>

        <Button
          onClick={startScan}
          className="mt-4 w-full bg-red-600 hover:bg-red-700 text-white font-bold text-lg py-2 rounded-xl"
          disabled={scanning}
        >
          {scanning ? "Escaneando..." : "¡Escanear!"}
        </Button>

        {status && (
          <motion.p
            initial={{ scale: 0 }}
            animate={{ scale: 1 }}
            className={`mt-6 text-3xl font-extrabold ${status === "VERDAD" ? "text-green-600" : "text-red-600"}`}
          >
            {status === "VERDAD" ? "✅ VERDAD" : "❌ MENTIRA"}
          </motion.p>
        )}

        <div className="mt-6 space-y-2">
          <p className="text-sm text-gray-600">Modo broma:</p>
          <div className="flex justify-center gap-2">
            <Button onClick={() => setForceResult("VERDAD")} className="bg-green-500 hover:bg-green-600">
              Forzar Verdad
            </Button>
            <Button onClick={() => setForceResult("MENTIRA")} className="bg-red-500 hover:bg-red-600">
              Forzar Mentira
            </Button>
            <Button onClick={() => setForceResult(null)} className="bg-gray-500 hover:bg-gray-600">
              Aleatorio
            </Button>
          </div>
        </div>
      </div>
    </main>
  );
}
