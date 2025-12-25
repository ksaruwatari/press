import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Checkbox } from "@/components/ui/checkbox";

// ①定型評価 ②総合評価 ③日時分類 ④規模分類 ⑤詳細ページ＋1回投票

const CRITERIA = [
  "事実確認が明確である",
  "論点が整理されている",
  "市民にとって知る価値が高い",
  "回答を引き出す構成になっている",
  "重複質問である",
  "前提が不明確である",
];

export default function PressConferenceReviewApp() {
  const [page, setPage] = useState("home"); // home | list | detail
  const [selectedConf, setSelectedConf] = useState(null);

  const [filters, setFilters] = useState({ level: "all", date: "" });

  const [conferences, setConferences] = useState([
    {
      id: 1,
      name: "内閣官房長官 定例記者会見",
      date: "2025-01-10",
      level: "国",
      questions: [
        {
          id: 1,
          text: "今回の政策変更の法的根拠を具体的に教えてください。",
          votesUp: 12,
          votesDown: 3,
          criteria: {},
          voted: false,
        },
        {
          id: 2,
          text: "国民の理解は得られているとお考えですか。",
          votesUp: 4,
          votesDown: 10,
          criteria: {},
          voted: false,
        },
      ],
    },
    {
      id: 2,
      name: "◯◯県知事 定例記者会見",
      date: "2025-01-08",
      level: "都道府県",
      questions: [
        {
          id: 1,
          text: "予算増額の財源はどこから確保しますか。",
          votesUp: 9,
          votesDown: 1,
          criteria: {},
          voted: false,
        },
      ],
    },
  ]);

  const voteOnce = (confId, qId, type) => {
    setConferences((prev) =>
      prev.map((c) =>
        c.id === confId
          ? {
              ...c,
              questions: c.questions.map((q) =>
                q.id === qId && !q.voted
                  ? {
                      ...q,
                      voted: true,
                      votesUp: type === "up" ? q.votesUp + 1 : q.votesUp,
                      votesDown:
                        type === "down" ? q.votesDown + 1 : q.votesDown,
                    }
                  : q
              ),
            }
          : c
      )
    );
  };

  const toggleCriteria = (confId, qId, label) => {
    setConferences((prev) =>
      prev.map((c) =>
        c.id === confId
          ? {
              ...c,
              questions: c.questions.map((q) =>
                q.id === qId
                  ? {
                      ...q,
                      criteria: { ...q.criteria, [label]: !q.criteria[label] },
                    }
                  : q
              ),
            }
          : c
      )
    );
  };

  const filtered = conferences.filter((c) => {
    if (filters.level !== "all" && c.level !== filters.level) return false;
    if (filters.date && c.date !== filters.date) return false;
    return true;
  });

  // ===== HOME =====
  if (page === "home") {
    return (
      <div className="p-6 max-w-5xl mx-auto text-center">
        <h1 className="text-4xl font-bold mb-4">記者会見 質問評価アーカイブ</h1>
        <p className="text-gray-600 mb-8">
          記者会見における「質問」を市民の視点で評価・可視化するプラットフォーム
        </p>
        <Button size="lg" onClick={() => setPage("list")}>
          会見一覧を見る
        </Button>
      </div>
    );
  }

  // ===== DETAIL =====
  if (page === "detail" && selectedConf) {
    const conf = conferences.find((c) => c.id === selectedConf);
    return (
      <div className="p-6 max-w-5xl mx-auto">
        <Button variant="outline" className="mb-4" onClick={() => setPage("list")}>
          ← 会見一覧へ戻る
        </Button>
        <h1 className="text-2xl font-bold mb-1">{conf.name}</h1>
        <p className="text-sm text-gray-600 mb-6">
          {conf.date} ／ 規模：{conf.level}
        </p>

        <div className="space-y-4">
          {conf.questions.map((q) => (
            <Card key={q.id} className="rounded-xl bg-gray-50">
              <CardContent className="p-4">
                <p className="mb-3">{q.text}</p>

                {/* ②総合評価 */}
                <div className="flex gap-3 mb-4">
                  <Button size="sm" disabled={q.voted} onClick={() => voteOnce(conf.id, q.id, "up")}>
                    👍 有用 {q.votesUp}
                  </Button>
                  <Button size="sm" variant="outline" disabled={q.voted} onClick={() => voteOnce(conf.id, q.id, "down")}>
                    👎 改善 {q.votesDown}
                  </Button>
                </div>

                {/* ①定型評価 */}
                <div className="grid grid-cols-2 gap-2 text-sm">
                  {CRITERIA.map((label) => (
                    <label key={label} className="flex items-center gap-2">
                      <Checkbox checked={!!q.criteria[label]} onCheckedChange={() => toggleCriteria(conf.id, q.id, label)} />
                      {label}
                    </label>
                  ))}
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
      </div>
    );
  }

  // ===== LIST =====
  return (
    <div className="p-6 max-w-6xl mx-auto">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-3xl font-bold">会見一覧</h1>
        <Button variant="outline" onClick={() => setPage("home")}>
          トップへ
        </Button>
      </div>

      {/* ③日時＋④規模フィルタ */}
      <div className="flex gap-4 mb-8">
        <select className="border p-2 rounded" value={filters.level} onChange={(e) => setFilters({ ...filters, level: e.target.value })}>
          <option value="all">すべての規模</option>
          <option value="国">国</option>
          <option value="都道府県">都道府県</option>
          <option value="市区町村">市区町村</option>
        </select>
        <input type="date" className="border p-2 rounded" value={filters.date} onChange={(e) => setFilters({ ...filters, date: e.target.value })} />
      </div>

      <div className="grid md:grid-cols-2 gap-6">
        {filtered.map((conf) => (
          <Card key={conf.id} className="rounded-2xl shadow cursor-pointer hover:shadow-lg" onClick={() => { setSelectedConf(conf.id); setPage("detail"); }}>
            <CardContent className="p-6">
              <h2 className="text-xl font-semibold mb-1">{conf.name}</h2>
              <p className="text-sm text-gray-600">{conf.date} ／ 規模：{conf.level}</p>
              <p className="text-sm mt-2 text-gray-500">質問数：{conf.questions.length}</p>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
