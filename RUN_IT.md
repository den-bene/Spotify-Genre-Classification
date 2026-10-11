# Eseguire il progetto

## Google Colab — percorso più semplice

1. Apri [Google Colab](https://colab.research.google.com/) e scegli **File → Carica notebook**. Seleziona `Spotify_10k_Tracks_Project.ipynb` da questa cartella.
2. Usa un runtime Python con CPU: la GPU non serve per questi modelli.
3. Scegli **Runtime → Esegui tutto**. La prima cella ti chiederà di caricare `10k_tracks.csv`, incluso nella repo, oppure disponibile nella cartella ML o nella sottocartella `data` della copia locale. Mantieni questo nome.
4. Attendi la ricerca dei parametri e le curve. Può richiedere diversi minuti. Non modificare i parametri dopo aver visto il test per cercare un punteggio più alto.
5. Salva il notebook eseguito con **File → Scarica → Scarica .ipynb**. Dal pannello File scarica anche `spotify_results.zip`; il modello salvato è in `models/spotify_genre_pipeline.joblib`.

I file del runtime sono temporanei: salvarli solo nel pannello File non li conserva dopo la chiusura. Puoi anche salvare il notebook su Drive dal menu File. Non serve montare Drive per eseguire il progetto.

Le librerie usate sono comuni in Colab. Se il runtime segnala una versione incompatibile, aggiungi temporaneamente una cella iniziale con:

```python
%pip install "numpy>=1.26,<3" "pandas>=2.2,<4" "scipy>=1.11,<2" "scikit-learn>=1.5,<2" "matplotlib>=3.8,<4" "joblib>=1.4,<2"
```

Poi riavvia il runtime e riesegui tutto. I dettagli del caricamento sono nella [guida ufficiale di Colab](https://colab.research.google.com/notebooks/io.ipynb).

## VS Code — alternativa locale

Apri **questa cartella** in VS Code con File → Apri cartella. Installa le estensioni Microsoft **Python** e **Jupyter**. Nel terminale integrato crea l'ambiente e installa le dipendenze:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Usa Python 3.11 o successivo. Apri il notebook, scegli in alto **Seleziona kernel → Ambienti Python → .venv**, poi **Esegui tutto**. Il CSV può restare nella cartella principale, come nella repo; è supportato anche `data/10k_tracks.csv`.

## Cosa presentare su GitHub

Il progetto analizza 13 variabili con uno split stratificato 80/20, preprocessing dentro la cross-validation e confronto di classificatori. La ricerca dei parametri di Random Forest privilegia la riduzione dell'overfitting mantenendo una performance di validation utile. I testi spiegano scelte, risultati e limiti; le tabelle riportano valori ottenuti durante l'esecuzione.

La repo essenziale contiene il notebook con gli output, `README.md`, questa guida, `requirements.txt`, `.gitignore` e `10k_tracks.csv`. Il README riporta autore, fonte e licenza CC BY-NC-SA 4.0 del dataset. Report, modello binario, ZIP e ambiente virtuale possono restare locali: vengono generati eseguendo il notebook.

Nel CV presentalo come progetto universitario di classificazione dei generi musicali con valutazione e controllo dell'overfitting. Evita di rivendicare un risultato massimo per Spotify o una validazione su artisti mai visti: lo split contiene artisti presenti anche nel training.
