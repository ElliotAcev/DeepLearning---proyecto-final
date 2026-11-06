# Propuesta de proyecto final — INFB6083 Deep Learning

**1. Identificación.** *Análisis multimodal de tumores cerebrales mediante Deep Learning.* Autor: Elliot. Repositorio: https://github.com/ElliotAcev/DeepLearning---proyecto-final/tree/main.

**2. Problema y para quién.** Un servicio de radiología que evalúa incorporar IA de apoyo debe decidir qué modelo adoptar para clasificar tumores cerebrales y delimitarlos. Hoy no puede compararlos con rigor: los resultados pueden inflarse por imágenes duplicadas y atajos como el brillo, y la delimitación manual de bordes es lenta y varía entre observadores. El proyecto es un benchmark comparativo (CNN propia, preentrenados y U-Net) con evaluación sin fuga de datos, que simula su valor clínico como apoyo bajo supervisión humana; no diagnostica.

**3. Tarea.** Pipeline: MRI → clasificación (glioma, meningioma, pituitario, sin tumor) → segmentación (U-Net) → características morfológicas → informe estructurado y visualización 3D.
- *Fase 1 (evaluada):* clasificación y segmentación.
- *Fase 2:* informe descriptivo (NLP) desde clase, máscara y características.
- *Fase 3 (opcional):* reconstrucción 3D, con volumen aproximado y dimensiones de la lesión.

Resultado esperado: benchmark comparativo entre modelo base y preentrenados, pipeline integral y valor clínico simulado (datos sintéticos).

**4. Corpus.** BRISC 2025: 6000 imágenes MRI, 4 clases, con 4793 pares imagen-máscara (bajo el techo de 50 000). Licencia: CC BY 4.0 (uso permitido con atribución, que cito). Tamaño en disco: 294 MB (bajo el techo de 500 MB). Descarga por código, sin subir datos al repo. Contra la fuga: deduplicación (md5 + phash), porque el test oficial repite imágenes del train; split estratificado 70/15/15 con semilla fija, cada imagen con su máscara en la misma partición.

**5. Escalera de modelos** (misma partición).
- *ML clásico:* regresión logística/SVM sobre HOG e histograma.
- *DL desde cero:* mi CNN propia (ya implementada: 4 bloques conv + BatchNorm) y U-Net desde cero.
- *Propuesto (transfer learning):* ResNet-18 preentrenada en ImageNet y U-Net con codificador preentrenado. Congelo las capas convolucionales, entreno la cabeza y luego ajusto el último bloque con tasa baja.

**6. Métricas.** Efectividad: accuracy, precision, recall y F1 macro (clasificación); Dice e IoU (segmentación). Eficiencia: tiempo de entrenamiento y latencia por imagen.

**7. Visualización interactiva.** App Streamlit local: subo una MRI y veo clase, Grad-CAM, máscara, medidas e informe; pestañas con matriz de confusión y comparación de modelos. Funciona sin red con los pesos guardados.

**8. Recursos de cómputo y de IA.** GPU NVIDIA RTX 4060 (8 GB); CPU y RAM: ryzen 5 5500, 32 GB RAM. Colab: plan gratis; entrenamiento en menos de 2 h de T4. Asistentes de IA: Claude y OpenCode (Ollama).

**9. Riesgo principal y plan B.** Alcance excesivo y que el modelo aprenda el brillo en vez de la anatomía. Plan B: priorizar clasificación + U-Net, informe con plantillas y 3D opcional.
