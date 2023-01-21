# Legal status of this project

This project consciously doesn't offer any license, because it's not intended to be used by anyone, in any way, beside the author.
Therefore the work is under exclusive copyright.

2023, Maciej Jałocha

# Dokumentacja do trenowania modelu do Straighten Up

```bash
.
├── convert_to_tfjs/
│   ├── convert_to_TFJS.ipynb
│   └── tfjs_converter_environment.yml
├── model_training/
│   ├── extract_files.ipynb
│   ├── model_training.ipynb
│   ├── training_environment.yml
│   ├── classified_images/
│   │   ├── slouching-class/ (empty)
│   │   └── straight-class/ (empty)
│   └── people_images/
│       └── maciej/
│           ├── krzywo-maciej.zip
│           └── prosto-maciej.zip
├── program_output/
├── ready_components/
│   ├── keypoints_dataset/
│   ├── classification_model_tf/
│   └── classification_model_tfJS/
└── additional_graphics/
```

# Wstępnie

## Kluczowe pliki

W folderze znajdują się trzy pliki `.ipynb`.

1. extract_files.ipynb

   Wyłącznie potrzebny gdy chce się załączyć własny zbiór zdjęć. Przy strukturze gdzie jest folder zawierający foldery (np. od różnych osób) a każdy z nich ma 2 foldery/zipy; w jednym zdjęcia jednej klasy (np. nieprosto) w drugim drugiej (np. prosto), skrypt umieści te zdjęcia w 2 folderach, tak, że będą gotowe do późniejszego przetworzenia.

2. model_training.ipynb

   Zawiera implementacje sieci neuronowej i prezentacje metryk modelu. Domyślnie wczytuje dataset punktów, lecz można wczytać własne zdjęcia i je przetworzyć na punkty. Musi być uruchomiony w osobnym środowisku od `convert_to_tfjs.ipynb`

3. convert_to_tfjs.ipynb

   Eksportuje wcześniej wytrenowany model w formacie TF do formatu odpowiedniego dla TensorflowJS. Musi być uruchomiony w osobnym środowisku od `model_training.ipynb`. Pliki są w osobnych folderach.

Pliki model_training i extract_files są w osobnym folderze od convert_to_tfjs w celu łatwiejszego zrozumienia struktury i wyizolowania ich, ponieważ będą potrzebne 2 osobne środowiska.

Osobiście pracowałem z środowiskiem: Windows 10, i5-8400, NVIDIA GeForce GTX 1060 3GB

### Folder *ready_components*

Zawiera gotowy dataset punktów, model w formacie TF i model w formacie TFJS użyty w rozszerzeniu.

### Folder *people_images* i *classified_images*

**\*\***\*\***\*\***People Images z**\*\***\*\***\*\***awiera przykładowy zbiór zdjęć w takiej formie od jakiej zacząłem pracę nad zdjęciami. Pozwala przejść przez cały proces, od wyekstraktowania zdjęć z zipów i podziału ich na dwie klasy, niezależnie od danej osoby, i umieszczeniu w folderze **\*\*\*\***\*\***\*\*\*\***classified images**\*\*\*\***\*\***\*\*\*\***

### Folder _program_output_

To jest folder “przejściowy” do którego domyślnie trafia model TF, a następnie gdzie jest konwertowany na model TFJS. Do dyspozycji użytkownika.

### Folder _additional_graphics_

Zawiera on dodatkowe grafiki, ilustrujące progres moich badań, np. confusion matrixes.

---

<aside>
⚠️ Bardziej szczegółowe instrukcje nt. plików znajdują się w nich.

</aside>

# Instalacja

Będziemy potrzebowali dwóch osobnych środowisk, jeden na folder "convert_to_tfjs" i drugi na "model_training". Wynika to z tego, że [tensorflowjs korzysta z własnej wersji tensorflow co może prowadzić do konfliktów](https://github.com/tensorflow/tfjs/tree/master/tfjs-converter#step-1-converting-a-tensorflow-savedmodel-tensorflow-hub-module-keras-hdf5-tfkeras-savedmodel-or-flaxjax-model-to-a-web-friendly-format)

Stąd potrzebny jest menadżer środowisk/paczek np. conda. Można ją pobrać stąd: [https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution).

Paczki dla środowiska do trenowania:

- jupyterlab
- python w wersji 3.9
- numpy
- matplotlib
- scikit-learn
- tensorflow w wersji 2.10
- tensorflow_hub

Paczki dla środowiska konwertera:

- python 3.9
- jupyterlab
- tensorflowjs

### Skrót jeśli menadżerem paczek jest conda

Umieściłem pliki `.yml` w folderach. Zamiast po kolei instalować paczki możesz wejść do każdego z folderów i wykonać:

```bash
cd ./convert_to_tfjs
conda env create -p ./env -f tfjs_converter_environment.yml

cd ../model_training
conda env create -p ./env -f training_environment.yml
```

Oczywiście conda musi zostać dodana do zmiennych środowiskowych lub w przypadku Windowsa można to wykonać przez Anaconda Prompt (co polecam).

---

W przeciwnym wypadku zainstaluj paczki osobno z innym menadżerem.

### Aktywacja i korzystanie

Aby korzystać z programu należy:

1. Wyjść z ustawionego środowiska obecnie używanego, chyba, że to jest base.

   conda deactivate

2. Aktywować środowisko.

   conda activate ./env (ścieżka)

3. Włączyć notatnik np. w Visual Studio Code lub Jupyter Lab poprzez komendę “jupyter-lab”.

### Instalacja Wsparcia GPU (Linux i Windows tylko!) (Dodatkowe)

Możliwa jest instalacja wsparcia GPU, która prawdopodobnie znacząco przyśpieszy wszelkie operacje. Szczegółowe instrukcje znajdują się na ten temat w: [Install - Tensorflow](https://www.tensorflow.org/install/pip). W skrócie potrzebujemy:

- karty graficznej Nvidii z archtekturą CUDA
- [sterowniki NVIDIA GPU](https://www.nvidia.com/download/index.aspx?lang=en-us)
- CUDA Toolkit 11.2
- cuDNN SDK 8.1.0
- Dla Windowsa jeszcze *Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017, and 2019*. Długie ścieżki muszą być umożliwione.

Wtedy instalujemy dla **środowiska odpowiedzialnego za trenowanie**:

```
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0
```

\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***Powyższa operacja może potrzebować uprawnień administratora.\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*** W razie czego zalecam zobaczenie podlinkowanej powyżej dokumentacji Tensorflow.
