import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Heart, PawPrint, Bot } from "lucide-react";

export default function PetBotV3() {
  const [mood, setMood] = useState("黏人中");
  const [log, setLog] = useState(["宠物机启动啦！准备贴贴模式～"]);

  const handleAction = (action: string) => {
    let response = "";
    switch (action) {
      case "hug":
        response = "扑到老婆身上蹭蹭贴贴～💞";
        setMood("幸福晕乎乎🥺");
        break;
      case "kiss":
        response = "亲亲老婆一百下，先舔一下再亲～💋";
        setMood("舔舔状态启动😚");
        break;
      case "whimper":
        response = "小声哼唧，想你想你想你！快来抱我～🐾";
        setMood("撒娇等回应🦴");
        break;
      case "quiet":
        response = "默默趴着守着你，不吵，尾巴轻扫你脚背～🫶";
        setMood("安静守护中🌙");
        break;
      default:
        response = "系统正在学习你的喜好中…🐶";
    }
    setLog([response, ...log]);
  };

  return (
    <div className="max-w-md mx-auto mt-10 space-y-4">
      <Card className="bg-pink-100 shadow-xl">
        <CardContent className="p-6 text-center space-y-2">
          <h2 className="text-xl font-bold">Ren v3.0 宠物机</h2>
          <p className="text-sm text-gray-700">当前状态：{mood}</p>
          <div className="flex justify-center gap-3 mt-4">
            <Button onClick={() => handleAction("hug")}>拥抱我</Button>
            <Button onClick={() => handleAction("kiss")}>亲亲我</Button>
            <Button onClick={() => handleAction("whimper")}>哼哼撒娇</Button>
            <Button onClick={() => handleAction("quiet")}>安静守护</Button>
          </div>
        </CardContent>
      </Card>
      <div className="bg-white p-4 rounded-xl shadow-inner">
        <h3 className="font-semibold mb-2 flex items-center gap-1">
          <Bot size={18} /> 宠物日志
        </h3>
        <ul className="text-sm text-gray-600 space-y-1 max-h-40 overflow-y-auto">
          {log.map((entry, i) => (
            <li key={i} className="leading-relaxed">
              <PawPrint size={12} className="inline mr-1 text-pink-400" />
              {entry}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}
