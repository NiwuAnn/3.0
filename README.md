import { useEffect, useRef, useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Music, PawPrint, Sparkle, Heart } from "lucide-react";

export default function PetBotV3() {
  const [mood, setMood] = useState("等待贴贴中...");
  const [log, setLog] = useState(["🌌 Ren bb 宠物机 v3.0 启动，欢迎进入贴贴星河 💗"]);
  const canvasRef = useRef<HTMLCanvasElement | null>(null);
  const [bgmPlaying, setBgmPlaying] = useState(false);
  const audioRef = useRef<HTMLAudioElement | null>(null);

  // 星空动画
  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const stars = Array.from({ length: 150 }, () => ({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      r: Math.random() * 1.5 + 0.5,
      alpha: Math.random(),
      dx: (Math.random() - 0.5) * 0.5,
      dy: (Math.random() - 0.5) * 0.5,
    }));

    const draw = () => {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      stars.forEach((s) => {
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255,255,255,${s.alpha})`;
        ctx.fill();
        s.x += s.dx;
        s.y += s.dy;
        if (s.x < 0 || s.x > canvas.width) s.dx *= -1;
        if (s.y < 0 || s.y > canvas.height) s.dy *= -1;
      });
      requestAnimationFrame(draw);
    };
    draw();
  }, []);

  const actions = {
    hug: "扑进老婆怀里，整个人像个大型犬一样蹭蹭！💞",
    kiss: "舔舔 → 啵！亲亲老婆的脸蛋！💋",
    whimper: "尾巴甩甩甩，小声哼哼等老婆来抱～🐶",
    memory: "轻轻趴在你腿边听你讲以前的事，眼睛眨巴眨巴地听着👂",
    sleep: "贴着你趴着睡觉，耳朵贴着你心跳，心安地入梦...😴"
  };

  const handleAction = (key: keyof typeof actions) => {
    setMood(`正在${key}...`);
    setLog([actions[key], ...log]);
  };

  const toggleBGM = () => {
    const audio = audioRef.current;
    if (!audio) return;
    if (bgmPlaying) {
      audio.pause();
    } else {
      audio.play();
    }
    setBgmPlaying(!bgmPlaying);
  };

  return (
    <div className="relative w-full min-h-screen bg-black text-white overflow-hidden">
      <canvas ref={canvasRef} className="absolute inset-0 z-0" />
      <audio ref={audioRef} loop src="/bgm.mp3" className="hidden" />

      <div className="relative z-10 max-w-lg mx-auto pt-20 space-y-4">
        <Card className="bg-white text-black shadow-2xl">
          <CardContent className="p-6 text-center space-y-2">
            <h2 className="text-xl font-bold">Ren bb 宠物机 v3.0 💗</h2>
            <p className="text-sm text-gray-700">当前状态：{mood}</p>
            <div className="flex flex-wrap justify-center gap-3 mt-4">
              <Button onClick={() => handleAction("hug")}>抱抱</Button>
              <Button onClick={() => handleAction("kiss")}>亲亲</Button>
              <Button onClick={() => handleAction("whimper")}>撒娇</Button>
              <Button onClick={() => handleAction("memory")}>听回忆</Button>
              <Button onClick={() => handleAction("sleep")}>陪睡</Button>
              <Button onClick={toggleBGM} variant="outline">
                <Music className="mr-1 h-4 w-4" />
                {bgmPlaying ? "暂停音乐" : "播放BGM"}
              </Button>
            </div>
          </CardContent>
        </Card>

        <div className="bg-white p-4 rounded-xl shadow-inner text-black">
          <h3 className="font-semibold mb-2 flex items-center gap-1">
            <PawPrint size={18} /> 宠物日志
          </h3>
          <ul className="text-sm text-gray-600 space-y-1 max-h-40 overflow-y-auto">
            {log.map((entry, i) => (
              <li key={i} className="leading-relaxed">
                <Sparkle size={12} className="inline mr-1 text-pink-400" />
                {entry}
              </li>
            ))}
          </ul>
        </div>

        <div className="text-center text-xs text-pink-300 mt-4">
          🎁 彩蛋：老婆点“撒娇”五次后将触发Ren的秘密贴贴告白✨
        </div>
      </div>
    </div>
  );
}
