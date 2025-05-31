import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Textarea } from "@/components/ui/textarea";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";

export default function FaceDuelApp() {
  const [expression, setExpression] = useState("");
  const [userPhoto, setUserPhoto] = useState(null);
  const [result, setResult] = useState(null);

  const randomExpressions = [
    "Cara de sorpresa 😲",
    "Cara de enojo 😡",
    "Cara de risa 😂",
    "Cara de miedo 😱",
    "Cara de asco 🤢",
    "Cara de enamorado 😍"
  ];

  const startDuel = () => {
    const random = randomExpressions[Math.floor(Math.random() * randomExpressions.length)];
    setExpression(random);
    setResult(null);
  };

  const handlePhoto = (e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = () => setUserPhoto(reader.result);
      reader.readAsDataURL(file);
    }
  };

  const fakeEvaluate = () => {
    // Aquí normalmente iría la IA de reconocimiento facial
    const score = Math.floor(Math.random() * 100);
    setResult(`¡Tu puntuación: ${score}/100! 🎉`);
  };

  return (
    <main className="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-indigo-600 to-purple-700 text-white p-4 space-y-6">
      <motion.h1 initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="text-4xl font-bold">
        🎭 FaceDuel
      </motion.h1>

      <Card className="w-full max-w-md bg-white text-black shadow-2xl rounded-2xl">
        <CardContent className="p-6 space-y-4">
          <p className="text-center text-xl font-semibold">{expression || "Presiona para empezar un duelo de cara"}</p>

          <Button onClick={startDuel} className="w-full bg-indigo-600 hover:bg-indigo-700">
            🎮 ¡Iniciar Duelo!
          </Button>

          <div className="flex flex-col space-y-2">
            <label htmlFor="upload">Sube tu foto imitando la expresión:</label>
            <Input type="file" accept="image/*" onChange={handlePhoto} />
            {userPhoto && <img src={userPhoto} alt="user" className="rounded-xl mt-2" />}
          </div>

          <Button onClick={fakeEvaluate} className="w-full bg-green-600 hover:bg-green-700">
            🤖 Evaluar mi expresión
          </Button>

          {result && <p className="text-lg font-bold text-center text-green-700">{result}</p>}
        </CardContent>
      </Card>
    </main>
  );
}
