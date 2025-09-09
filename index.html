import React, { useState, useCallback, useRef } from 'react';
import { FileText, User, Target, Clock, Users, Settings, BookOpen, Download, Sparkles, ChevronDown, ChevronUp, Upload, X, FileCheck, Instagram, AlertCircle, Key } from 'lucide-react';

const SpeechTherapyGenerator = () => {
  const [formData, setFormData] = useState({
    userDescription: '',
    specificObjective: '',
    duration: '',
    sessionType: 'individual',
    isPediatric: false,
    additionalContext: {
      customContext: ''
    },
    references: []
  });
  
  const [generatedActivity, setGeneratedActivity] = useState(null);
  const [isGenerating, setIsGenerating] = useState(false);
  const [showAdvanced, setShowAdvanced] = useState(false);
  const [uploadedFiles, setUploadedFiles] = useState([]);
  const [apiKey, setApiKey] = useState('');
  const [showApiKeyInput, setShowApiKeyInput] = useState(false);
  const [error, setError] = useState('');
  const [processingFiles, setProcessingFiles] = useState(false);
  const fileInputRef = useRef(null);

  const bloomLevels = [
    { level: 'Recordar', description: 'Reconocer, listar, describir, identificar', color: 'bg-red-100 text-red-800' },
    { level: 'Comprender', description: 'Interpretar, resumir, inferir, parafrasear', color: 'bg-orange-100 text-orange-800' },
    { level: 'Aplicar', description: 'Ejecutar, implementar, demostrar, utilizar', color: 'bg-yellow-100 text-yellow-800' },
    { level: 'Analizar', description: 'Diferenciar, organizar, relacionar, comparar', color: 'bg-green-100 text-green-800' },
    { level: 'Evaluar', description: 'Revisar, formular hipótesis, criticar, experimentar', color: 'bg-blue-100 text-blue-800' },
    { level: 'Crear', description: 'Generar, planear, producir, diseñar', color: 'bg-purple-100 text-purple-800' }
  ];

  const handleInputChange = (field, value) => {
    if (field.includes('.')) {
      const [parent, child] = field.split('.');
      setFormData(prev => ({
        ...prev,
        [parent]: {
          ...prev[parent],
          [child]: value
        }
      }));
    } else {
      setFormData(prev => ({
        ...prev,
        [field]: value
      }));
    }
  };

  // PDF processing function
  const processPDFContent = async (file) => {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = async (e) => {
        try {
          const arrayBuffer = e.target.result;
          const uint8Array = new Uint8Array(arrayBuffer);
          
          // Simple PDF text extraction - in production, you'd use a proper PDF library
          // This is a basic implementation that extracts visible text
          const text = await extractTextFromPDF(uint8Array);
          resolve(text);
        } catch (error) {
          reject(error);
        }
      };
      reader.onerror = () => reject(new Error('Error reading file'));
      reader.readAsArrayBuffer(file);
    });
  };

  // Basic PDF text extraction (simplified)
  const extractTextFromPDF = async (uint8Array) => {
    try {
      // Convert to string and look for text between common PDF text markers
      const str = new TextDecoder('latin1').decode(uint8Array);
      const textPattern = /BT\s*(.*?)\s*ET/g;
      let extractedText = '';
      let match;
      
      while ((match = textPattern.exec(str)) !== null) {
        const textContent = match[1]
          .replace(/\/\w+\s+\d+(\.\d+)?\s+Tf/g, '') // Remove font declarations
          .replace(/\d+(\.\d+)?\s+\d+(\.\d+)?\s+Td/g, '') // Remove positioning
          .replace(/\([^)]*\)\s*Tj/g, (m) => {
            // Extract text from Tj operators
            return m.replace(/^\(/, '').replace(/\)\s*Tj$/, '') + ' ';
          })
          .replace(/\[[^\]]*\]\s*TJ/g, (m) => {
            // Extract text from TJ operators  
            return m.replace(/^\[/, '').replace(/\]\s*TJ$/, '').replace(/\([^)]*\)/g, (text) => {
              return text.replace(/^\(/, '').replace(/\)$/, '') + ' ';
            });
          });
        
        extractedText += textContent + '\n';
      }
      
      return extractedText.trim() || 'Unable to extract readable text from this PDF. Please ensure the PDF contains selectable text.';
    } catch (error) {
      return 'Error processing PDF content. The file may be corrupted or contain only images.';
    }
  };

  const handleFileUpload = useCallback(async (event) => {
    const files = Array.from(event.target.files);
    const validFiles = files.filter(file => 
      file.type === 'application/pdf' && file.size <= 50 * 1024 * 1024 // 50MB max
    );
    
    if (validFiles.length !== files.length) {
      setError('Solo se permiten archivos PDF con un tamaño máximo de 50MB');
      setTimeout(() => setError(''), 5000);
      return;
    }

    setProcessingFiles(true);
    const processedFiles = [];

    try {
      for (const file of validFiles) {
        const extractedText = await processPDFContent(file);
        processedFiles.push({
          file,
          extractedText,
          processed: true
        });
      }

      setUploadedFiles(prev => [...prev, ...processedFiles]);
      setFormData(prev => ({
        ...prev,
        references: [...prev.references, ...processedFiles]
      }));
    } catch (error) {
      setError(`Error procesando archivos: ${error.message}`);
      setTimeout(() => setError(''), 5000);
    } finally {
      setProcessingFiles(false);
      if (fileInputRef.current) {
        fileInputRef.current.value = '';
      }
    }
  }, []);

  const removeFile = (index) => {
    setUploadedFiles(prev => prev.filter((_, i) => i !== index));
    setFormData(prev => ({
      ...prev,
      references: prev.references.filter((_, i) => i !== index)
    }));
  };

  // Gemini AI integration
  const callGeminiAPI = async (prompt) => {
    if (!apiKey) {
      throw new Error('API key de Gemini es requerida');
    }

    const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent?key=${apiKey}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        contents: [{
          parts: [{
            text: prompt
          }]
        }],
        generationConfig: {
          temperature: 0.7,
          topK: 40,
          topP: 0.95,
          maxOutputTokens: 8192,
        }
      })
    });

    if (!response.ok) {
      const errorData = await response.json().catch(() => ({}));
      throw new Error(`Error de API: ${response.status} - ${errorData.error?.message || 'Error desconocido'}`);
    }

    const data = await response.json();
    
    if (!data.candidates || !data.candidates[0] || !data.candidates[0].content) {
      throw new Error('Respuesta inválida de la API de Gemini');
    }

    return data.candidates[0].content.parts[0].text;
  };

  const buildGeminiPrompt = (formData, contextInfo) => {
    const referencesText = formData.references
      .map((ref, index) => `\n--- Referencia ${index + 1}: ${ref.file.name} ---\n${ref.extractedText}`)
      .join('\n');

    return `
Eres un experto fonoaudiólogo especializado en crear actividades terapéuticas. Genera una actividad personalizada basada en los siguientes parámetros:

INFORMACIÓN DEL PACIENTE:
${formData.userDescription}

OBJETIVO ESPECÍFICO:
${formData.specificObjective}

DURACIÓN: ${formData.duration} minutos
TIPO DE SESIÓN: ${formData.sessionType}
MODALIDAD PEDIÁTRICA: ${formData.isPediatric ? 'Sí - usar lenguaje lúdico adaptado' : 'No - usar lenguaje profesional estándar'}

CONTEXTO ADICIONAL:
${formData.additionalContext.customContext || 'No especificado'}

${referencesText ? `REFERENCIAS CIENTÍFICAS PARA FUNDAMENTAR:\n${referencesText}` : ''}

INSTRUCCIONES DE FORMATO:
Devuelve la respuesta en formato JSON válido con la siguiente estructura:

{
  "title": "Título de la actividad",
  "smartObjective": "Objetivo SMART completo y específico",
  "description": "Descripción detallada de la actividad",
  "materials": ["Lista", "de", "materiales", "necesarios"],
  "procedure": [
    {
      "name": "Nombre de la fase",
      "time": número_de_minutos,
      "description": "Descripción detallada de esta fase",
      "color": "bg-blue-100 border-blue-200 text-blue-800"
    }
  ],
  "evaluation": {
    "criteria": "Criterios de evaluación específicos",
    "methods": ["Métodos", "de", "evaluación"],
    "feedback": "Tipo de retroalimentación a proporcionar"
  },
  "adaptations": ["Lista", "de", "adaptaciones", "específicas"],
  "theoreticalFoundation": ["Bases", "teóricas", "y", "científicas"]
}

REQUERIMIENTOS ESPECÍFICOS:
1. Usar metodología SMART para el objetivo
2. Integrar principios de la Taxonomía de Bloom
3. Incluir 3 fases en el procedimiento (calentamiento 15%, desarrollo 65%, cierre 20%)
4. Si es pediátrico, usar lenguaje motivador y lúdico
5. Si hay referencias científicas, incorporarlas en la fundamentación teórica
6. Adaptar materiales y estrategias al contexto específico proporcionado
7. Asegurar que la duración total coincida con lo solicitado

Genera una actividad profesional, creativa y fundamentada científicamente.
`;
  };

  const generateActivity = async () => {
    if (!apiKey) {
      setError('Por favor, ingresa tu API key de Gemini');
      setTimeout(() => setError(''), 5000);
      return;
    }

    setIsGenerating(true);
    setError('');

    try {
      const age = parseInt(formData.userDescription.match(/\d+/)?.[0] || 60);
      const contextInfo = analyzeAdditionalContext(formData.additionalContext.customContext);
      
      const prompt = buildGeminiPrompt(formData, contextInfo);
      const response = await callGeminiAPI(prompt);
      
      // Parse JSON response
      let activityData;
      try {
        // Remove any markdown formatting if present
        const cleanResponse = response.replace(/```json\n?|\n?```/g, '').trim();
        activityData = JSON.parse(cleanResponse);
      } catch (parseError) {
        throw new Error('Error parseando la respuesta de la IA. Intenta de nuevo.');
      }

      setGeneratedActivity(activityData);
    } catch (error) {
      setError(`Error generando actividad: ${error.message}`);
      setTimeout(() => setError(''), 10000);
    } finally {
      setIsGenerating(false);
    }
  };

  const analyzeAdditionalContext = (contextText) => {
    if (!contextText) return {};
    const context = contextText.toLowerCase();
    const info = {};

    // Material type detection
    const materialKeywords = {
      'visual': ['tarjetas visuales', 'imágenes', 'pictogramas', 'láminas', 'fotos', 'dibujos', 'visual'],
      'auditivo': ['grabaciones', 'música', 'sonidos', 'audio', 'canciones', 'melodías', 'auditivo'],
      'táctil': ['texturas', 'objetos', 'táctil', 'manipulativo', 'concreto', 'palpar', 'tocar'],
      'digital': ['aplicaciones', 'apps', 'tablet', 'computadora', 'software', 'digital', 'tecnología', 'dispositivo']
    };

    for (const [type, keywords] of Object.entries(materialKeywords)) {
      if (keywords.some(keyword => context.includes(keyword))) {
        info.materialType = type;
        break;
      }
    }

    return info;
  };

  const exportActivity = () => {
    if (!generatedActivity) return;

    const content = `
ACTIVIDAD FONOAUDIOLÓGICA GENERADA CON IA
=========================================

${generatedActivity.title}

OBJETIVO SMART:
${generatedActivity.smartObjective}

DESCRIPCIÓN:
${generatedActivity.description}

MATERIALES:
${generatedActivity.materials.map(m => `• ${m}`).join('\n')}

PROCEDIMIENTO:
${generatedActivity.procedure.map(p => 
  `${p.name} (${p.time} min):\n${p.description}\n`
).join('\n')}

EVALUACIÓN:
Criterio: ${generatedActivity.evaluation.criteria}

Métodos:
${generatedActivity.evaluation.methods.map(m => `• ${m}`).join('\n')}

Retroalimentación: ${generatedActivity.evaluation.feedback}

ADAPTACIONES:
${generatedActivity.adaptations.map(a => `• ${a}`).join('\n')}

FUNDAMENTACIÓN TEÓRICA:
${generatedActivity.theoreticalFoundation.map(f => `• ${f}`).join('\n')}

---
Generado con IA - ${new Date().toLocaleDateString()}
    `;
    
    const blob = new Blob([content], { type: 'text/plain;charset=utf-8' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `actividad_fonoaudiologica_${new Date().toISOString().split('T')[0]}.txt`;
    a.click();
    URL.revokeObjectURL(url);
  };

  return (
    <div className="max-w-7xl mx-auto p-6 bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50 min-h-screen relative">
      {/* Error Alert */}
      {error && (
        <div className="fixed top-4 left-1/2 transform -translate-x-1/2 bg-red-500 text-white px-6 py-3 rounded-lg shadow-lg z-50 flex items-center gap-2">
          <AlertCircle className="w-5 h-5" />
          <span>{error}</span>
          <button onClick={() => setError('')} className="ml-2 text-white hover:text-gray-200">
            <X className="w-4 h-4" />
          </button>
        </div>
      )}

      {/* Watermark */}
      <div className="absolute bottom-4 right-4 text-xs text-gray-400 font-medium z-10">
        <div className="flex items-center gap-1">
          <span>Creado por</span>
          <span className="font-bold text-indigo-600">Flgo. Cristóbal San Martín</span>
          <a 
            href="https://instagram.com/flgo_crissanmartin" 
            target="_blank" 
            rel="noopener noreferrer"
            className="flex items-center gap-1 hover:text-indigo-600 transition-colors"
          >
            <Instagram className="w-4 h-4" />
            @flgo_crissanmartin
          </a>
        </div>
      </div>

      <div className="bg-white rounded-2xl shadow-2xl overflow-hidden border border-gray-100">
        {/* Header */}
        <div className="bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 text-white p-8 relative overflow-hidden">
          <div className="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent animate-pulse"></div>
          <div className="relative z-10">
            <div className="flex items-center gap-4 mb-4">
              <div className="p-3 bg-white/20 rounded-full backdrop-blur-sm">
                <Sparkles className="w-8 h-8" />
              </div>
              <div>
                <h1 className="text-3xl md:text-4xl font-bold bg-gradient-to-r from-white to-gray-200 bg-clip-text text-transparent">
                  Generador IA de Actividades Fonoaudiológicas
                </h1>
                <p className="text-indigo-100 text-lg mt-1">Potenciado con Gemini AI para Fonoaudiólog@s</p>
              </div>
            </div>
            
            {/* API Key Section */}
            <div className="mb-6">
              <button
                onClick={() => setShowApiKeyInput(!showApiKeyInput)}
                className="flex items-center gap-2 bg-white/10 hover:bg-white/20 px-4 py-2 rounded-lg transition-all duration-200"
              >
                <Key className="w-5 h-5" />
                <span className="text-sm font-medium">
                  {apiKey ? 'API Key Configurada' : 'Configurar API Key Gemini'}
                </span>
                {showApiKeyInput ? <ChevronUp className="w-4 h-4" /> : <ChevronDown className="w-4 h-4" />}
              </button>
              
              {showApiKeyInput && (
                <div className="mt-3 p-4 bg-white/10 rounded-lg backdrop-blur-sm">
                  <input
                    type="password"
                    placeholder="Ingresa tu API Key de Google Gemini"
                    value={apiKey}
                    onChange={(e) => setApiKey(e.target.value)}
                    className="w-full p-3 rounded-lg bg-white/20 placeholder-indigo-200 text-white border border-white/30 focus:border-white focus:outline-none"
                  />
                  <p className="text-indigo-100 text-xs mt-2">
                    Obtén tu API key gratuita en: https://aistudio.google.com/app/apikey
                  </p>
                </div>
              )}
            </div>

            <div className="flex flex-wrap gap-4 p-4 bg-white/10 rounded-xl backdrop-blur-sm">
              <div className="flex items-center gap-2">
                <Target className="w-5 h-5" />
                <span className="text-sm font-medium">Objetivos SMART</span>
              </div>
              <div className="flex items-center gap-2">
                <BookOpen className="w-5 h-5" />
                <span className="text-sm font-medium">Taxonomía Bloom</span>
              </div>
              <div className="flex items-center gap-2">
                <Users className="w-5 h-5" />
                <span className="text-sm font-medium">Adaptación Pediátrica</span>
              </div>
              <div className="flex items-center gap-2">
                <Sparkles className="w-5 h-5" />
                <span className="text-sm font-medium">IA Gemini</span>
              </div>
            </div>
          </div>
        </div>

        <div className="grid lg:grid-cols-2 gap-8 p-8">
          {/* Form */}
          <div className="space-y-8">
            {/* User Information */}
            <div className="bg-gradient-to-br from-blue-50 to-indigo-50 rounded-xl p-6 border border-blue-100">
              <h2 className="text-xl font-bold mb-5 flex items-center gap-3 text-gray-800">
                <div className="p-2 bg-blue-600 text-white rounded-lg">
                  <User className="w-5 h-5" />
                </div>
                Información del Usuario
              </h2>
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-semibold text-gray-700 mb-2">
                    Descripción del usuario (edad en meses y contexto clínico)
                  </label>
                  <textarea
                    value={formData.userDescription}
                    onChange={(e) => handleInputChange('userDescription', e.target.value)}
                    placeholder="Ej: Niño de 48 meses con retraso en el desarrollo del lenguaje expresivo, presenta dificultades en la articulación de fonemas fricativos"
                    className="w-full p-4 border border-gray-300 rounded-xl focus:ring-4 focus:ring-blue-200 focus:border-blue-500 transition-all duration-200 resize-none"
                    rows="4"
                  />
                </div>
                <div className="flex items-center gap-3 p-4 bg-white rounded-xl border border-gray-200 hover:border-blue-300 transition-all duration-200">
                  <input
                    type="checkbox"
                    id="pediatric"
                    checked={formData.isPediatric}
                    onChange={(e) => handleInputChange('isPediatric', e.target.checked)}
                    className="w-5 h-5 text-blue-600 focus:ring-blue-500 border-gray-300 rounded cursor-pointer"
                  />
                  <label htmlFor="pediatric" className="text-sm font-medium text-gray-800 cursor-pointer">
                    Sesión Pediátrica (lenguaje lúdico adaptado)
                  </label>
                </div>
              </div>
            </div>

            {/* Objectives and Session */}
            <div className="bg-gradient-to-br from-green-50 to-emerald-50 rounded-xl p-6 border border-green-100">
              <h2 className="text-xl font-bold mb-5 flex items-center gap-3 text-gray-800">
                <div className="p-2 bg-green-600 text-white rounded-lg">
                  <Target className="w-5 h-5" />
                </div>
                Objetivo y Sesión
              </h2>
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-semibold text-gray-700 mb-2">
                    Objetivo específico (basado en Taxonomía de Bloom)
                  </label>
                  <input
                    value={formData.specificObjective}
                    onChange={(e) => handleInputChange('specificObjective', e.target.value)}
                    placeholder="Ej: Mejorar la articulación del fonema /r/ en palabras bisílabas con un 80% de precisión"
                    className="w-full p-4 border border-gray-300 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all duration-200"
                  />
                  <div className="mt-3 p-3 bg-white rounded-lg border border-gray-200">
                    <div className="flex flex-wrap gap-2">
                      {bloomLevels.map((level, index) => (
                        <span key={index} className={`px-3 py-1 rounded-full text-xs font-medium ${level.color}`}>
                          {level.level}
                        </span>
                      ))}
                    </div>
                    <p className="text-xs text-gray-600 mt-2">
                      <strong>Progresión:</strong> De Recordar a Crear - Desarrollo cognitivo estructurado
                    </p>
                  </div>
                </div>
                <div className="grid grid-cols-2 gap-4">
                  <div>
                    <label className="block text-sm font-semibold text-gray-700 mb-2">
                      Duración (minutos)
                    </label>
                    <input
                      type="number"
                      value={formData.duration}
                      onChange={(e) => handleInputChange('duration', e.target.value)}
                      placeholder="30"
                      min="15"
                      max="120"
                      className="w-full p-4 border border-gray-300 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all duration-200"
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-semibold text-gray-700 mb-2">
                      Tipo de sesión
                    </label>
                    <select
                      value={formData.sessionType}
                      onChange={(e) => handleInputChange('sessionType', e.target.value)}
                      className="w-full p-4 border border-gray-300 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all duration-200 bg-white"
                    >
                      <option value="individual">Individual</option>
                      <option value="grupal">Grupal</option>
                      <option value="hogar">Para el hogar</option>
                    </select>
                  </div>
                </div>
              </div>
            </div>

            {/* Additional Context */}
            <div className="bg-gradient-to-br from-purple-50 to-pink-50 rounded-xl p-6 border border-purple-100">
              <button
                onClick={() => setShowAdvanced(!showAdvanced)}
                className="flex items-center justify-between w-full text-xl font-bold text-gray-800 hover:text-purple-600 transition-colors mb-5"
              >
                <span className="flex items-center gap-3">
                  <div className="p-2 bg-purple-600 text-white rounded-lg">
                    <Settings className="w-5 h-5" />
                  </div>
                  Contexto Adicional (Opcional)
                </span>
                {showAdvanced ? <ChevronUp className="w-6 h-6" /> : <ChevronDown className="w-6 h-6" />}
              </button>
              
              {showAdvanced && (
                <div className="mt-4">
                  <div>
                    <label className="block text-sm font-semibold text-gray-700 mb-3">
                      Contexto adicional detallado
                    </label>
                    <textarea
                      value={formData.additionalContext.customContext}
                      onChange={(e) => handleInputChange('additionalContext.customContext', e.target.value)}
                      placeholder="Describe materiales específicos, estrategias terapéuticas, entorno de intervención, adaptaciones necesarias, intereses del paciente, etc."
                      className="w-full p-4 border border-gray-300 rounded-xl focus:ring-4 focus:ring-purple-200 focus:border-purple-500 transition-all duration-200 text-sm resize-none"
                      rows="6"
                    />
                  </div>
                </div>
              )}
            </div>

            {/* File Upload */}
            <div className="bg-gradient-to-br from-orange-50 to-red-50 rounded-xl p-6 border border-orange-100">
              <h2 className="text-xl font-bold mb-5 flex items-center gap-3 text-gray-800">
                <div className="p-2 bg-orange-600 text-white rounded-lg">
                  <BookOpen className="w-5 h-5" />
                </div>
                Referencias Científicas (Opcional)
              </h2>
              <div className="space-y-4">
                <div 
                  className={`border-2 border-dashed border-orange-300 rounded-xl p-6 text-center hover:border-orange-400 transition-colors cursor-pointer ${processingFiles ? 'opacity-50' : ''}`}
                  onClick={() => !processingFiles && fileInputRef.current?.click()}
                >
                  {processingFiles ? (
                    <div className="flex items-center justify-center gap-3">
                      <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-orange-600"></div>
                      <span className="text-orange-600 font-medium">Procesando archivos PDF...</span>
                    </div>
                  ) : (
                    <>
                      <Upload className="w-10 h-10 text-orange-400 mx-auto mb-3" />
                      <span className="block text-sm font-medium text-gray-700 mb-1">
                        Subir archivos PDF (máx. 50MB cada uno)
                      </span>
                      <span className="text-xs text-gray-500">
                        Los PDFs serán procesados automáticamente para extraer contenido científico
                      </span>
                    </>
                  )}
                  <input
                    ref={fileInputRef}
                    type="file"
                    multiple
                    accept=".pdf"
                    onChange={handleFileUpload}
                    className="hidden"
                    disabled={processingFiles}
                  />
                </div>
                
                {uploadedFiles.length > 0 && (
                  <div className="space-y-3">
                    <h4 className="text-sm font-semibold text-gray-700">Archivos procesados:</h4>
                    {uploadedFiles.map((fileData, index) => (
                      <div key={index} className="bg-white p-4 rounded-lg border border-gray-200 hover:border-orange-300 transition-colors">
                        <div className="flex items-start justify-between gap-3">
                          <div className="flex-1">
                            <div className="flex items-center gap-3 mb-2">
                              <FileCheck className="w-5 h-5 text-green-600" />
                              <span className="text-sm font-medium text-gray-800">{fileData.file.name}</span>
                              <span className="text-xs text-gray-500">({(fileData.file.size / 1024 / 1024).toFixed(1)} MB)</span>
                            </div>
                            <div className="bg-gray-50 p-3 rounded-lg">
                              <p className="text-xs text-gray-600 mb-1">Contenido extraído:</p>
                              <p className="text-xs text-gray-700 line-clamp-3">
                                {fileData.extractedText.substring(0, 200)}...
                              </p>
                            </div>
                          </div>
                          <button
                            onClick={() => removeFile(index)}
                            className="p-2 text-red-500 hover:text-red-700 hover:bg-red-50 rounded-lg transition-all duration-200 flex-shrink-0"
                            title="Eliminar archivo"
                          >
                            <X className="w-4 h-4" />
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            </div>

            {/* Generate Button */}
            <button
              onClick={generateActivity}
              disabled={!formData.userDescription || !formData.specificObjective || !formData.duration || !apiKey || isGenerating}
              className="w-full bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 text-white py-5 px-8 rounded-xl font-bold text-lg hover:from-indigo-700 hover:via-purple-700 hover:to-pink-700 disabled:opacity-50 disabled:cursor-not-allowed transition-all duration-300 transform hover:scale-105 active:scale-95 shadow-lg flex items-center justify-center gap-3"
            >
              {isGenerating ? (
                <>
                  <div className="animate-spin rounded-full h-6 w-6 border-b-2 border-white"></div>
                  <span>Generando con Gemini AI...</span>
                </>
              ) : (
                <>
                  <Sparkles className="w-6 h-6" />
                  <span>Generar Actividad con IA</span>
                </>
              )}
            </button>
          </div>

          {/* Results */}
          <div className="lg:sticky lg:top-8">
            {generatedActivity ? (
              <div className="bg-white border border-gray-200 rounded-2xl shadow-xl overflow-hidden hover:shadow-2xl transition-shadow duration-300">
                <div className="bg-gradient-to-r from-green-600 to-emerald-600 text-white p-6 flex justify-between items-start">
                  <div>
                    <h2 className="text-2xl font-bold">{generatedActivity.title}</h2>
                    <p className="text-green-100 text-sm mt-1 flex items-center gap-2">
                      <Sparkles className="w-4 h-4" />
                      Generado con Gemini AI
                    </p>
                  </div>
                  <button
                    onClick={exportActivity}
                    className="bg-white/20 hover:bg-white/30 p-3 rounded-xl transition-all duration-200 flex items-center gap-2 group"
                    title="Exportar actividad"
                  >
                    <Download className="w-5 h-5 group-hover:scale-110 transition-transform duration-200" />
                    <span className="text-sm font-medium">Exportar</span>
                  </button>
                </div>
                
                <div className="p-8 max-h-[70vh] overflow-y-auto space-y-8">
                  <div className="bg-blue-50 p-5 rounded-xl border-l-4 border-blue-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <Target className="w-5 h-5 text-blue-600" />
                      Objetivo SMART:
                    </h3>
                    <p className="text-gray-700 leading-relaxed">{generatedActivity.smartObjective}</p>
                  </div>

                  <div className="bg-gray-50 p-5 rounded-xl border-l-4 border-gray-400">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <FileText className="w-5 h-5 text-gray-600" />
                      Descripción:
                    </h3>
                    <p className="text-gray-700 leading-relaxed">{generatedActivity.description}</p>
                  </div>

                  <div className="bg-yellow-50 p-5 rounded-xl border-l-4 border-yellow-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <Users className="w-5 h-5 text-yellow-600" />
                      Materiales:
                    </h3>
                    <ul className="space-y-2">
                      {generatedActivity.materials.map((material, index) => (
                        <li key={index} className="flex items-start gap-3">
                          <span className="text-yellow-600 mt-1 flex-shrink-0">•</span>
                          <span className="text-gray-700">{material}</span>
                        </li>
                      ))}
                    </ul>
                  </div>

                  <div className="bg-green-50 p-5 rounded-xl border-l-4 border-green-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <Clock className="w-5 h-5 text-green-600" />
                      Procedimiento:
                    </h3>
                    <div className="space-y-4">
                      {generatedActivity.procedure.map((phase, index) => (
                        <div key={index} className={`p-4 rounded-lg border-l-4 ${phase.color || 'bg-gray-100 border-gray-200 text-gray-800'}`}>
                          <div className="font-bold text-gray-800 mb-2 flex justify-between items-center">
                            <span>{phase.name}</span>
                            <span className="bg-white/30 px-3 py-1 rounded-full text-sm font-medium">
                              {phase.time} min
                            </span>
                          </div>
                          <p className="text-gray-700">{phase.description}</p>
                        </div>
                      ))}
                    </div>
                  </div>

                  <div className="bg-purple-50 p-5 rounded-xl border-l-4 border-purple-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <Settings className="w-5 h-5 text-purple-600" />
                      Evaluación:
                    </h3>
                    <div className="space-y-3">
                      <div className="bg-white p-3 rounded-lg border border-purple-200">
                        <p className="text-sm font-medium text-gray-800 mb-1">Criterio:</p>
                        <p className="text-gray-700">{generatedActivity.evaluation.criteria}</p>
                      </div>
                      <div className="bg-white p-3 rounded-lg border border-purple-200">
                        <p className="text-sm font-medium text-gray-800 mb-2">Métodos:</p>
                        <ul className="space-y-2">
                          {generatedActivity.evaluation.methods.map((method, index) => (
                            <li key={index} className="flex items-start gap-3">
                              <span className="text-purple-600 mt-1 flex-shrink-0">•</span>
                              <span className="text-gray-700">{method}</span>
                            </li>
                          ))}
                        </ul>
                      </div>
                      <div className="bg-white p-3 rounded-lg border border-purple-200">
                        <p className="text-sm font-medium text-gray-800 mb-1">Retroalimentación:</p>
                        <p className="text-gray-700">{generatedActivity.evaluation.feedback}</p>
                      </div>
                    </div>
                  </div>

                  <div className="bg-pink-50 p-5 rounded-xl border-l-4 border-pink-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <Sparkles className="w-5 h-5 text-pink-600" />
                      Adaptaciones:
                    </h3>
                    <ul className="space-y-2">
                      {generatedActivity.adaptations.map((adaptation, index) => (
                        <li key={index} className="flex items-start gap-3">
                          <span className="text-pink-600 mt-1 flex-shrink-0">•</span>
                          <span className="text-gray-700">{adaptation}</span>
                        </li>
                      ))}
                    </ul>
                  </div>

                  <div className="bg-indigo-50 p-5 rounded-xl border-l-4 border-indigo-500">
                    <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                      <BookOpen className="w-5 h-5 text-indigo-600" />
                      Fundamentación Teórica:
                    </h3>
                    <ul className="space-y-2">
                      {generatedActivity.theoreticalFoundation.map((foundation, index) => (
                        <li key={index} className="flex items-start gap-3">
                          <span className="text-indigo-600 mt-1 flex-shrink-0">•</span>
                          <span className="text-gray-700">{foundation}</span>
                        </li>
                      ))}
                    </ul>
                  </div>

                  {formData.references.length > 0 && (
                    <div className="bg-gray-50 p-5 rounded-xl border-l-4 border-gray-400">
                      <h3 className="font-bold text-lg text-gray-800 mb-3 flex items-center gap-2">
                        <FileText className="w-5 h-5 text-gray-600" />
                        Referencias Procesadas:
                      </h3>
                      <div className="bg-white p-4 rounded-lg border border-gray-200">
                        <ul className="space-y-2">
                          {formData.references.map((fileData, index) => (
                            <li key={index} className="flex items-center gap-3 p-2 bg-gray-50 rounded">
                              <FileText className="w-5 h-5 text-gray-500" />
                              <span className="text-gray-700 font-medium">{fileData.file.name}</span>
                              <span className="text-xs text-green-600 bg-green-100 px-2 py-1 rounded">Procesado</span>
                            </li>
                          ))}
                        </ul>
                        <p className="text-xs text-gray-500 mt-3 italic">
                          * Las referencias fueron procesadas y su contenido incorporado en la fundamentación teórica.
                        </p>
                      </div>
                    </div>
                  )}
                </div>
              </div>
            ) : (
              <div className="bg-gradient-to-br from-gray-50 to-gray-100 rounded-2xl border-2 border-dashed border-gray-300 p-12 text-center hover:border-gray-400 transition-colors">
                <div className="mb-6">
                  <div className="inline-block p-4 bg-white rounded-full shadow-lg">
                    <Sparkles className="w-12 h-12 text-gray-400" />
                  </div>
                </div>
                <h3 className="text-2xl font-bold text-gray-700 mb-3">
                  Actividad Lista para Generar
                </h3>
                <p className="text-gray-600 text-lg max-w-md mx-auto mb-8">
                  Configura tu API key de Gemini, completa los campos requeridos y haz clic en "Generar Actividad" para crear una actividad fonoaudiológica personalizada con IA.
                </p>
                
                <div className="space-y-6">
                  <div className="bg-white p-5 rounded-xl shadow-sm border border-gray-200">
                    <h4 className="font-bold text-lg text-gray-800 mb-4 flex items-center gap-2">
                      <BookOpen className="w-5 h-5 text-orange-600" />
                      Taxonomía de Bloom - Niveles Cognitivos:
                    </h4>
                    <div className="grid grid-cols-1 gap-3">
                      {bloomLevels.map((level, index) => (
                        <div key={index} className="flex justify-between items-center p-3 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
                          <span className="font-bold text-lg text-blue-600">{level.level}:</span>
                          <span className="text-gray-700 text-right max-w-xs">{level.description}</span>
                        </div>
                      ))}
                    </div>
                  </div>
                  
                  <div className="bg-white p-5 rounded-xl shadow-sm border border-gray-200">
                    <h4 className="font-bold text-lg text-gray-800 mb-4 flex items-center gap-2">
                      <Target className="w-5 h-5 text-green-600" />
                      Metodología SMART:
                    </h4>
                    <div className="text-left space-y-2">
                      <div className="flex items-start gap-3 p-3 bg-green-50 rounded-lg">
                        <span className="bg-green-600 text-white px-2 py-1 rounded font-bold text-sm">S</span>
                        <div>
                          <strong className="text-gray-800">Específico</strong>
                          <p className="text-gray-600 text-sm">Objetivo claro y bien definido</p>
                        </div>
                      </div>
                      <div className="flex items-start gap-3 p-3 bg-blue-50 rounded-lg">
                        <span className="bg-blue-600 text-white px-2 py-1 rounded font-bold text-sm">M</span>
                        <div>
                          <strong className="text-gray-800">Medible</strong>
                          <p className="text-gray-600 text-sm">Criterios cuantificables</p>
                        </div>
                      </div>
                      <div className="flex items-start gap-3 p-3 bg-yellow-50 rounded-lg">
                        <span className="bg-yellow-600 text-white px-2 py-1 rounded font-bold text-sm">A</span>
                        <div>
                          <strong className="text-gray-800">Alcanzable</strong>
                          <p className="text-gray-600 text-sm">Realista según capacidades</p>
                        </div>
                      </div>
                      <div className="flex items-start gap-3 p-3 bg-purple-50 rounded-lg">
                        <span className="bg-purple-600 text-white px-2 py-1 rounded font-bold text-sm">R</span>
                        <div>
                          <strong className="text-gray-800">Relevante</strong>
                          <p className="text-gray-600 text-sm">Pertinente al caso clínico</p>
                        </div>
                      </div>
                      <div className="flex items-start gap-3 p-3 bg-red-50 rounded-lg">
                        <span className="bg-red-600 text-white px-2 py-1 rounded font-bold text-sm">T</span>
                        <div>
                          <strong className="text-gray-800">Temporal</strong>
                          <p className="text-gray-600 text-sm">Con plazos definidos</p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            )}
          </div>
        </div>

        {/* Footer */}
        <div className="bg-gradient-to-r from-gray-50 to-gray-100 border-t border-gray-200 p-6">
          <div className="flex flex-col md:flex-row justify-between items-center gap-4">
            <div className="flex items-center gap-3">
              <Sparkles className="w-5 h-5 text-indigo-600" />
              <span className="font-semibold text-gray-700">Generador IA de Actividades Fonoaudiológicas</span>
            </div>
            <div className="flex flex-wrap gap-6 text-sm text-gray-600">
              <div className="flex items-center gap-2">
                <Target className="w-4 h-4 text-green-600" />
                <span>Objetivos SMART</span>
              </div>
              <div className="flex items-center gap-2">
                <BookOpen className="w-4 h-4 text-orange-600" />
                <span>Taxonomía Bloom</span>
              </div>
              <div className="flex items-center gap-2">
                <Users className="w-4 h-4 text-purple-600" />
                <span>Adaptación Pediátrica</span>
              </div>
              <div className="flex items-center gap-2">
                <Sparkles className="w-4 h-4 text-pink-600" />
                <span>Gemini AI</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default SpeechTherapyGenerator;
