import React from 'react';
import axios from 'axios';

export default function SpeakButton({ onTranscribed }) {
  async function handleFileUpload(ev) {
    const file = ev.target.files?.[0];
    if (!file) return;
    const fd = new FormData();
    fd.append('audio', file);
    try {
      const resp = await axios.post('/api/stt/file', fd, { headers: { 'Content-Type': 'multipart/form-data' } });
      onTranscribed(resp.data.transcription);
    } catch (err) {
      alert('STT failed: ' + (err.response?.data?.error || err.message));
    }
  }

  return (
    <div>
      <label style={{ display: 'flex', gap: 8, alignItems: 'center' }}>
        <input type="file" accept="audio/*" onChange={handleFileUpload} />
        <button type="button">Upload audio (or record locally)</button>
      </label>
      <p>Tip: Use browser recorder or phone voice memo then upload the file.</p>
    </div>
  );
}
