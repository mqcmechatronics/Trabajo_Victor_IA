# 5. Conclusiones

1. La integración de los datos históricos de Fórmula 1 resultó exitosa, consolidando información proveniente de 14 archivos CSV que abarcan el período 1950-2024. El dataset final estuvo conformado por 25 146 registros y 39 variables, proporcionando una base sólida para el análisis y modelado de datos.

2. El análisis exploratorio permitió identificar patrones históricos relevantes dentro del campeonato. Entre los hallazgos más importantes destaca que la posición de salida mantiene una fuerte relación con el resultado final, observándose que los pilotos que parten desde la pole position obtuvieron la victoria en el 48.1 % de las carreras y alcanzaron el podio en el 74.8 % de los casos analizados.

3. El modelo de agrupamiento K-Means presentó el mejor desempeño entre las técnicas evaluadas, obteniendo un coeficiente de silueta de 0.320. La segmentación permitió clasificar a los pilotos en cuatro grupos representativos de desempeño, diferenciando claramente campeones, ganadores frecuentes, pilotos de zona media y pilotos de bajo rendimiento.

4. El modelo de clasificación Random Forest logró un F1-score de 0.677 y un ROC-AUC de 0.938 sobre temporadas no utilizadas durante el entrenamiento, superando el desempeño de la línea base considerada. Estos resultados muestran que las variables históricas utilizadas contienen información suficiente para estimar con una precisión aceptable la probabilidad de alcanzar el podio.

5. La estrategia híbrida implementada para el asistente conversacional obtuvo una exactitud del 94.1 % (32 respuestas correctas de 34 preguntas), superando ampliamente el enfoque basado únicamente en texto-a-SQL, que alcanzó un 67.6 %. Esto evidencia la efectividad de combinar herramientas de consulta estructurada con modelos de lenguaje.

6. La integración de SQLite, MLflow, Ollama, Flask y Streamlit permitió desarrollar una solución funcional y reproducible, capaz de ejecutarse en una computadora personal sin requerir infraestructura especializada, demostrando la viabilidad de implementar soluciones completas de inteligencia artificial utilizando recursos locales.
