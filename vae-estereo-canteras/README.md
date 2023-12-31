# IA PARA EL MANTENIMIENTO PREDICTIVO EN CANTERAS: MODELADO

#### Autores: Fernando Marcos, Rodrigo Tamaki, Mateo Cámara, Viginia Yagüe y José Luis Blanco

### Resumen: 

#### La dependencia de materias primas, especialmente en el sector minero, es una pieza clave en la economía actual. Los agregados son vitales, siendo la segunda materia prima más usada después del agua. Transformar digitalmente este sector es clave para optimizar las operaciones. Sin embargo, la supervisión y mantenimiento (predictivo y correctivo) son desafíos poco explorados en este sector, debido a las particularidades del sector, la maquinaria y las condiciones ambientes. Todo ello, a pesar de los éxitos alcanzados en la monitorización con sensores acústicos y de contacto

#### Presentamos un esquema de aprendizaje no supervisado que entrena un modelo de autocodificador variacional sobre un conjunto de registros de sonido. Se trata del primer conjunto de datos de estas características recabado durante las operaciones de las plantas de procesamiento, y que contiene información de distintos puntos de la línea de procesado.

#### Los resultados evidencian la capacidad del modelo para reconstruir y representar en el espacio latente los sonidos registrados, las diferencias en las condiciones de operación y entre los distintos equipos. En el futuro, esto debe facilitar la clasificación de los sonidos, así como la detección de anomalías y patrones de degradación en el funcionamiento de la maquinaria.

### Abstract: 

#### Dependence on raw materials, especially in the mining sector, is a key part of today's economy. Aggregates are vital, being the second most used raw material after water. Digitally transforming this sector is key to optimizing operations. However, supervision and maintenance (predictive and corrective) are challenges little explored in this sector, due to the particularities of the sector, machinery and environmental conditions. All this, despite the successes achieved in other scenarios in monitoring with acoustic and contact sensors.

#### We present an unsupervised learning scheme that trains a variational autoencoder model on a set of sound records. This is the first such dataset collected during processing plant operations, containing information from different points of the processing line. 

####  Our results demonstrate the model's ability to reconstruct and represent in latent space the recorded sounds, the differences in operating conditions and between different equipment. In the future, this should facilitate the classification of sounds, as well as the detection of anomalies and degradation patterns in the operation of the machinery.

## Proyecciones latentes: Modelo 1
![Local Image](./images/PCA_tSNE_model1-coloured.png)

## Proyecciones latentes: Modelo 2
![Local Image](./images/PCA_tSNE_model2-coloured.png)

## Proyecciones latentes: Modelo 3
![Local Image](./images/PCA_tSNE_model3-coloured.png)


## Algunos ejemplos de reconstrucción VAE

<div class="figure">
    <table>
        <tbody>
            <!-- Row 1 -->
            <tr>
                <th>Grabación (id)</th>
                <td><b>Original</b></td>
                <td><b>Modelo 1</b></td>
                <td><b>Modelo 2</b></td>
                <td><b>Modelo 3</b></td>
            </tr>
            <!-- Row 2 -->
            <tr>
                <th>1</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_002_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_002_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_002_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_002_5.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 3 -->
            <tr>
                <th>2</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_003_10.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_003_10.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_003_10.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_003_10.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 4 -->
            <tr>
                <th>3</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_004_84.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_004_84.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_004_84.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_004_84.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 5 -->
            <tr>
                <th>4</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_005_271.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_005_271.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_005_271.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_005_271.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 6 -->
            <tr>
                <th>5</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_006_1030.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_006_1030.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_006_1030.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_006_1030.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 7 -->
            <tr>
                <th>6</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_007_31.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_007_31.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_007_31.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_007_31.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 8 -->
            <tr>
                <th>7</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_008_125.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_008_125.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_008_125.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_008_125.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 9 -->
            <tr>
                <th>8</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_009_180.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_009_180.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_009_180.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_009_180.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 10 -->
            <tr>
                <th>9</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_010_4.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_010_4.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_010_4.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_010_4.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 11 -->
            <tr>
                <th>10</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_011_185.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_011_185.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_011_185.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_011_185.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 12 -->
            <tr>
                <th>11</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_012_171.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_012_171.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_012_171.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_012_171.wav">
                    </audio>
                </td>
            </tr>
            <!-- Row 13 -->
            <tr>
                <th>Test</th>
                <td>
                    <audio controls="">
                        <source src="samples_original/frag_013_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model1/frag_013_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model2/frag_013_5.wav">
                    </audio>
                </td>
                <td>
                    <audio controls="">
                        <source src="samples_model3/frag_013_5.wav">
                    </audio>
                </td>
            </tr>
        </tbody>
    </table>
</div>
