# ⚛️ Classificazione Classica e Quantistica sul Dataset MNIST

## Panoramica del Progetto

Questo progetto si concentra sulla **classificazione binaria** (distinzione tra le cifre "0" e "1") del celebre dataset MNIST, offrendo un confronto tra approcci di Machine Learning tradizionali e modelli di **Quantum Machine Learning (QML)**, in particolare i Classificatori Variazionali Quantistici (VQC) e le Quantum Convolutional Neural Networks (QCNN).

L'obiettivo è testare l'efficacia degli algoritmi quantistici in un contesto a risorse limitate, a causa delle attuali restrizioni hardware e computazionali.

---

## 💾 Preprocessing e Riduzione del Dataset

Per rendere il problema affrontabile con gli attuali simulatori quantistici, è stata necessaria una drastica riduzione della dimensionalità:

1.  **Binary Classification:** Il task è stato limitato alla distinzione tra le sole cifre **"0" e "1"**.
2.  **Riduzione Dimensionale:** Le immagini originali **$28 \times 28$ pixel** (784 feature) sono state ridotte a **$3 \times 3$ pixel** (9 feature).
3.  **Metodo di Riduzione:** L'immagine è stata suddivisa in $3 \times 3$ blocchi, e per ciascuno è stata calcolata la **media dei pixel**.
4.  **Codifica:** Questo permette di codificare l'input in un circuito quantistico con **9 qubit**.

---

## 💻 1. Classificazione Classica (Benchmark)

Sono stati addestrati due modelli di Machine Learning supervisionato per stabilire un benchmark di performance:

* **SVC** (Support Vector Classifier)
* **Random Forest (RF)**

Entrambi i modelli hanno raggiunto un tasso di accuratezza del **100%** sia sul dataset completo ($28 \times 28$) che su quello compattato ($3 \times 3$).

* **Interpretazione (RF):** L'analisi dell'importanza delle feature ha mostrato che i modelli identificano facilmente i **pixel periferici** come cruciali, distinguendo la forma tondeggiante dello 0 dalla verticalità dell'1.

---

## ⚛️ 2. Classificazione Quantistica (QML)

Sono state esplorate due architetture quantistiche ibride, ottimizzate con l'algoritmo **COBYLA** (Constrained Optimization BY Linear Approximations).

### Variational Quantum Classifier (VQC)

Il VQC è composto da una mappa di codifica dati (Feature Map) e una rete parametrizzata (Ansatz).

| Architettura | Codifica (Feature Map) | Accuracy (Test Set) | Note |
| :--- | :--- | :--- | :--- |
| **VQC (ZFeatureMap)** | Rotazioni RZ (solo fase) | $0.683$ | Encoding privo di entanglement, con Ansatz RealAmplitudes. |
| **VQC (RY Encoding)** | Rotazioni RY (sulle ampiezze) | $\mathbf{0.733}$ | Codifica più flessibile e in grado di massimizzare l'informazione misurabile. |

* **Ottimizzatore:** **COBYLA** è risultato il più efficace in entrambi i VQC, essendo *derivative-free* (senza gradiente) e robusto in contesti quantistici rumorosi e con paesaggi di costo irregolari.

### Quantum Convolutional Neural Network (QCNN)

La QCNN è stata implementata ispirandosi alle reti neurali convoluzionali classiche, con l'obiettivo di estrarre pattern gerarchici tramite livelli di convoluzione e pooling quantistici.

* **Prestazioni:** Le prestazioni della QCNN sono risultate **inferiori** rispetto ai VQC più semplici.
* **Motivazione:** L'input estremamente ridotto ($3 \times 3$ pixel) non possiede la complessità strutturale necessaria per sfruttare appieno l'architettura gerarchica della QCNN. L'elevato numero di parametri la rende anche più difficile da ottimizzare.

---

## 🔬 Conclusioni

I risultati confermano che:

1.  **I modelli classici** (SVC, RF) ottengono un'accuratezza perfetta grazie alla semplicità intrinseca del problema binario (0 vs 1).
2.  **I modelli quantistici** (VQC) mostrano performance inferiori (Accuracy massima di $\sim 0.73$). Questo è atteso, data l'immaturità della tecnologia QML.
3.  **L'architettura QCNN**, sebbene teoricamente promettente, non è riuscita a esprimere il suo potenziale su un input così semplificato.
4.  Il modello **VQC (RY Encoding + RealAmplitudes)** ha fornito il miglior equilibrio tra espressività ed efficienza di ottimizzazione per questo specifico sottoinsieme del dataset MNIST.
