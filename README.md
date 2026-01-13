
import React, { useState } from 'react';
import { Lock, Map, Crown, Sparkles, ArrowRight, Plane, Search } from 'lucide-react';

const App = () => {
  const [currentLevel, setCurrentLevel] = useState(0);
  const [inputValue, setInputValue] = useState('');
  const [error, setError] = useState('');
  const [showFinalReveal, setShowFinalReveal] = useState(false);
  const [imageRevealed, setImageRevealed] = useState(false); // Nuevo estado para el "Toca para ver"

  // Definición de las tarjetas/niveles
  const levels = [
    {
      id: 1,
      theme: 'spy',
      front: {
        title: "ARCHIVO DE MISIÓN: CUMPLEAÑOS 28",
        subtitle: "AGENTE ACTIVADA: MARINA",
        body: "Se le ha asignado un objetivo clasificado como 'Nivel Máximo de Magia'. Para proceder a la extracción, debe desbloquear las coordenadas superando cuatro pruebas de inteligencia.",
        stamp: "ALTO SECRETO"
      },
      back: {
        title: "PRUEBA DE ACCESO 1: LA LLAVE",
        body: "Agente, el objetivo no se encuentra en territorio nacional. No es la vuelta a la esquina, ni una simple escapada en coche. Prepara tu equipaje más importante, porque vamos a cruzar fronteras.",
        question: "¿Qué documento 'azul' y esencial debes localizar inmediatamente para poder embarcar?",
        answers: ['pasaporte', 'el pasaporte', 'dni', 'el dni'],
        placeholder: "Escribe el documento..."
      }
    },
    {
      id: 2,
      theme: 'adventure',
      front: {
        title: "ACCESO CONCEDIDO",
        subtitle: "NIVEL 1 SUPERADO",
        body: "Procediendo a la siguiente fase de encriptación. Calibrando satélites...",
        stamp: "CONFIRMADO"
      },
      back: {
        title: "PRUEBA GEOGRÁFICA 2: EL DESTINO",
        body: "Calibremos la brújula. Nos dirigimos a un país famoso mundialmente por tres cosas: el 'l’amour', el vino excelente y una dama de hierro gigante en su capital. Pero el objetivo es un 'reino' vecino.",
        question: "¿Hacia qué país estamos orientando el rumbo?",
        answers: ['francia', 'france'],
        placeholder: "Escribe el país..."
      }
    },
    {
      id: 3,
      theme: 'princess',
      front: {
        title: "RUMBO FIJADO: FRANCIA",
        subtitle: "MAGIA DETECTADA",
        body: "Te acercas peligrosamente a la verdad. Procediendo a la identificación visual del objetivo.",
        stamp: "ROYAL"
      },
      back: {
        title: "PRUEBA ARQUITECTÓNICA 3: EL PALACIO",
        body: "En el epicentro exacto de nuestro destino se alza una estructura imponente de torres rosas y tejados azules. Se dice que pertenece a una joven princesa que durmió durante mucho tiempo...",
        question: "¿De qué estructura estamos hablando?",
        answers: ['castillo', 'el castillo', 'castillo de la bella durmiente', 'palacio'],
        placeholder: "Escribe la estructura..."
      }
    },
    {
      id: 4,
      theme: 'disney',
      front: {
        title: "ALERTA DE INTRUSIÓN",
        subtitle: "ESTÁS ARDIENDO, MARINA",
        body: "Solo queda una última confirmación de identidad de los locales para desbloquear tu regalo.",
        stamp: "MÁGICO"
      },
      back: {
        title: "PRUEBA FINAL 4: LOS JEFES",
        body: "ÉL: Orejas redondas, pantalones rojos. ELLA: Lazo de lunares. SU EQUIPO: Un pato con mal genio y un perro naranja alegre.",
        question: "Si sabes quiénes son, sabes dónde vamos. ¿CUÁL ES TU RESPUESTA FINAL?",
        answers: ['disney', 'disneyland', 'disneyland paris', 'eurodisney', 'mickey', 'mickey mouse'],
        placeholder: "¡Gritalo o escríbelo!"
      }
    }
  ];

  const handleNext = () => {
    const cleanInput = inputValue.trim().toLowerCase();
    const currentAnswers = levels[currentLevel].back.answers;

    if (currentAnswers.some(ans => cleanInput.includes(ans))) {
      setInputValue('');
      setError('');
      if (currentLevel < levels.length - 1) {
        setCurrentLevel(currentLevel + 1);
      } else {
        setShowFinalReveal(true);
      }
    } else {
      setError('Respuesta incorrecta, Agente. Inténtalo de nuevo.');
      const input = document.getElementById('answer-input');
      if(input) {
        input.classList.add('animate-shake');
        setTimeout(() => input.classList.remove('animate-shake'), 500);
      }
    }
  };

  const getThemeStyles = (theme) => {
    switch (theme) {
      case 'spy': return 'bg-slate-900 text-green-400 font-mono border-green-500';
      case 'adventure': return 'bg-amber-100 text-amber-900 font-serif border-amber-800';
      case 'princess': return 'bg-pink-100 text-pink-600 font-serif border-pink-400';
      case 'disney': return 'bg-red-600 text-white font-sans border-yellow-400';
      default: return 'bg-white text-black';
    }
  };

  const getIcon = (theme) => {
    switch (theme) {
      case 'spy': return <Lock className="w-12 h-12 mb-4" />;
      case 'adventure': return <Map className="w-12 h-12 mb-4" />;
      case 'princess': return <Crown className="w-12 h-12 mb-4" />;
      case 'disney': return <Sparkles className="w-12 h-12 mb-4 text-yellow-300" />;
      default: return <Lock />;
    }
  };

  // PANTALLA FINAL DE REVELACIÓN
  if (showFinalReveal) {
    return (
      <div className="min-h-screen bg-sky-900 flex flex-col items-center justify-center p-4 text-center overflow-hidden relative">
        <div className="absolute top-10 left-10 animate-pulse text-yellow-300"><Sparkles size={48}/></div>
        <div className="absolute bottom-20 right-10 animate-bounce text-pink-400"><Sparkles size={32}/></div>
        
        <div className="bg-white/10 backdrop-blur-md p-8 rounded-2xl border-2 border-yellow-400 max-w-lg w-full shadow-2xl z-10">
          <h1 className="text-4xl md:text-6xl font-bold text-yellow-400 mb-6 drop-shadow-md">
            ¡FELIZ CUMPLEAÑOS!
          </h1>
          
          <div className="bg-white p-4 rounded-lg shadow-inner mb-6 transform rotate-1 hover:rotate-0 transition-transform duration-500">
             
             {/* IMPORTANTE: 
                Para usar tu foto real, cambia la URL de abajo por: 'Feliz_28.jpg'
                y asegúrate de que la foto esté en la misma carpeta que este archivo.
             */}
             <div 
                onClick={() => setImageRevealed(true)}
                className="bg-sky-200 h-64 rounded flex items-center justify-center relative overflow-hidden cursor-pointer group"
             >
                <img 
                  // AQUÍ ESTÁ EL CAMBIO: Usamos una imagen web para la demo. 
                  // Cambia esto a: src="Feliz_28.jpg"
                  src="https://x.cdrst.com/foto/portal/1900/base/disney-paris-1900.jpg"
                  alt="Sorpresa" 
                  className={`w-full h-full object-cover transition-all duration-1000 ${imageRevealed ? 'blur-0 scale-100' : 'blur-xl scale-110'}`}
                />
                
                {/* Capa oscura que desaparece al hacer click */}
                <div className={`absolute inset-0 bg-black/80 flex flex-col items-center justify-center transition-opacity duration-700 ${imageRevealed ? 'opacity-0 pointer-events-none' : 'opacity-100'}`}>
                    <Sparkles className="text-yellow-400 w-12 h-12 mb-2 animate-spin-slow" />
                    <p className="text-white font-bold text-2xl animate-pulse">¡TOCA PARA VER!</p>
                    <p className="text-white/70 text-sm mt-2">(Pide un deseo antes...)</p>
                </div>
             </div>

          </div>

          <p className="text-xl text-white mb-8 leading-relaxed">
            Prepara las maletas y ponte las orejas...
            <br />
            <span className="text-3xl font-bold text-pink-300 mt-2 block">¡NOS VAMOS A DISNEYLAND!</span>
          </p>

          <button 
            onClick={() => window.location.reload()}
            className="px-6 py-3 bg-yellow-400 text-red-700 font-bold rounded-full hover:bg-yellow-300 transition-colors shadow-lg"
          >
            Volver a jugar
          </button>
        </div>
      </div>
    );
  }

  const currentData = levels[currentLevel];
  const themeClass = getThemeStyles(currentData.theme);

  return (
    <div className={`min-h-screen flex items-center justify-center p-4 transition-colors duration-1000 ${
      currentData.theme === 'spy' ? 'bg-black' : 
      currentData.theme === 'adventure' ? 'bg-amber-50' :
      currentData.theme === 'princess' ? 'bg-pink-50' : 'bg-red-700'
    }`}>
      <div className={`max-w-md w-full rounded-xl shadow-2xl overflow-hidden border-4 transition-all duration-500 ${themeClass}`}>
        <div className="p-6 border-b-2 border-dashed border-opacity-30 relative">
          <div className="absolute top-4 right-4 opacity-50 border-2 px-2 py-1 transform -rotate-12 text-xs font-bold uppercase tracking-widest">
            {currentData.front.stamp}
          </div>
          <div className="flex justify-center">
            {getIcon(currentData.theme)}
          </div>
          <h2 className="text-xl font-bold text-center mb-1">{currentData.front.title}</h2>
          <p className="text-center text-sm opacity-80 mb-4">{currentData.front.subtitle}</p>
          <p className="text-center italic text-sm">{currentData.front.body}</p>
        </div>

        <div className="p-6 bg-opacity-20 bg-black/5">
          <h3 className="font-bold text-lg mb-3 flex items-center gap-2">
            {currentLevel === 3 ? <Sparkles size={18}/> : <Search size={18}/>}
            {currentData.back.title}
          </h3>
          <p className="mb-6 leading-relaxed text-sm md:text-base">
            {currentData.back.body}
          </p>
          <div className="bg-white/90 p-4 rounded-lg shadow-inner text-gray-800">
            <p className="font-bold mb-3 text-center text-indigo-900">{currentData.back.question}</p>
            <input
              id="answer-input"
              type="text"
              value={inputValue}
              onChange={(e) => setInputValue(e.target.value)}
              placeholder={currentData.back.placeholder}
              className="w-full p-3 border-2 border-gray-300 rounded-md mb-2 focus:outline-none focus:border-indigo-500 transition-colors text-center font-sans text-lg"
              onKeyDown={(e) => e.key === 'Enter' && handleNext()}
            />
            {error && <p className="text-red-500 text-xs text-center font-bold animate-pulse">{error}</p>}
            <button
              onClick={handleNext}
              className={`w-full mt-3 py-3 rounded-lg font-bold flex items-center justify-center gap-2 transition-all transform active:scale-95 ${
                currentData.theme === 'disney' 
                  ? 'bg-yellow-400 text-black hover:bg-yellow-300' 
                  : 'bg-indigo-600 text-white hover:bg-indigo-700'
              }`}
            >
              {currentLevel === 3 ? '¡DESBLOQUEAR REGALO!' : 'DESENCRIPTAR'} <ArrowRight size={18} />
            </button>
          </div>
        </div>
        <div className="p-2 bg-black/10 flex justify-center gap-2">
          {levels.map((l, idx) => (
            <div 
              key={l.id} 
              className={`h-2 w-full rounded-full transition-all duration-500 ${
                idx <= currentLevel ? (currentData.theme === 'spy' ? 'bg-green-500' : 'bg-white') : 'bg-gray-400/30'
              }`}
            />
          ))}
        </div>
      </div>
      <style jsx global>{`
        @keyframes shake {
          0%, 100% { transform: translateX(0); }
          25% { transform: translateX(-5px); }
          75% { transform: translateX(5px); }
        }
        .animate-shake { animation: shake 0.3s ease-in-out; }
        .animate-spin-slow { animation: spin 3s linear infinite; }
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
      `}</style>
    </div>
  );
};

export default App;
