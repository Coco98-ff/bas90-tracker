// Fichier de diagnostic temporaire : à supprimer une fois le problème réglé.
module.exports = async (req, res) => {
  const key = process.env.GEMINI_API_KEY || '';
  const model = process.env.GEMINI_MODEL || 'gemini-2.5-flash';
  const out = { cle: { presente: !!key, longueur: key.length, commence_par_AIza: key.startsWith('AIza'), espaces_ou_guillemets: /[\s"']/.test(key) }, modele: model };
  const essai = async cfg => {
    try {
      const r = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json', 'x-goog-api-key': key.trim() },
        body: JSON.stringify({ contents: [{ parts: [{ text: 'Réponds par le mot OK.' }] }], generationConfig: cfg })
      });
      const j = await r.json().catch(() => ({}));
      return { status: r.status, message: ((j.error || {}).message || 'ok').slice(0, 200) };
    } catch (e) { return { status: 0, message: String(e.message).slice(0, 200) }; }
  };
  out.essai_comme_le_relais = await essai({ responseMimeType: 'application/json', thinkingConfig: { thinkingBudget: 0 } });
  out.essai_simple = await essai({});
  res.setHeader('Cache-Control', 'no-store');
  res.status(200).json(out);
};
