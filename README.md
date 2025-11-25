# 🚀 Auto-Install SD-Scripts dla Chroma1-HD Training

Pełna automatyzacja instalacji i treningu modeli LoRA dla Chroma1-HD na RTX 4090.

## 📋 Wymagania

- **OS**: Linux (Ubuntu/Debian) lub WSL2
- **Python**: 3.10 lub nowszy
- **GPU**: RTX 4090 (lub inna karta NVIDIA z CUDA)
- **CUDA**: 12.1
- **Dysk**: ~30GB wolnego miejsca
- **Git**: zainstalowany

## ⚡ Szybki Start (Instalacja)

### Opcja 1: Bezpośrednie pobranie plików

```bash
# Pobierz skrypt instalacyjny
wget https://raw.githubusercontent.com/r0wer/autotrainchro/main/setup.sh
chmod +x setup.sh

# Uruchom instalację (wszystko się zainstaluje automatycznie)
./setup.sh
```

### Opcja 2: Clone całego repo

```bash
git clone https://github.com/r0wer/autotrainchro.git
cd autotrainchro
chmod +x setup.sh
./setup.sh
```

## 📂 Struktura po instalacji

```
.
├── sd-scripts/                          # Główny folder (utworzony przez setup.sh)
│   ├── venv/                            # Środowisko wirtualne Python
│   ├── workspace/
│   │   ├── Chroma1-HD.safetensors       # Model bazowy
│   │   ├── t5xxl_fp16.safetensors       # Text encoder
│   │   ├── ae.safetensors               # AutoEncoder
│   │   ├── lora_config.toml             # Konfiguracja datasetu
│   │   ├── datasets/
│   │   │   └── goal/                    # ← TU dodaj zdjęcia + captions
│   │   ├── output/
│   │   │   └── chroma_loras/            # ← Wytrenowane modele
│   │   └── logs/                        # Logi TensorBoard
│   ├── train.sh                         # Skrypt treningu
│   └── menu.sh                          # Interaktywne menu
```

## 🎯 Użytkowanie

### Krok 1: Przygotuj dataset

Po instalacji, dodaj swoje zdjęcia treningowe:

```bash
cd sd-scripts/workspace/datasets/goal/
```

**Struktura datasetu:**
- Każde zdjęcie musi mieć odpowiadający plik `.txt` z opisem
- Przykład:
  ```
  photo1.jpg → photo1.txt
  photo2.png → photo2.txt
  ```

**Przykładowy caption (photo1.txt):**
```
a photo of a cat sitting on a red chair, detailed fur, professional lighting
```

### Krok 2: Uruchom trening

#### Metoda 1: Menu interaktywne (ZALECANE)

```bash
cd sd-scripts
./menu.sh
```

Menu oferuje:
- `1` - Rozpocznij trening LoRA
- `2` - Zobacz dataset (lista plików)
- `3` - Otwórz folder z wytrenowanymi modelami
- `4` - Zobacz logi TensorBoard
- `5` - Informacje o systemie
- `0` - Wyjście

#### Metoda 2: Bezpośredni trening

```bash
cd sd-scripts
./train.sh
```

## 🔧 Konfiguracja

### Zmiana parametrów treningu

Edytuj `workspace/lora_config.toml`:

```toml
[[datasets]]
resolution = [512, 512]    # Rozdzielczość (może być [768, 768] dla lepszej jakości)
batch_size = 2             # Zwiększ do 4 jeśli masz więcej VRAM
num_repeats = 10           # Ile razy powtórzyć każdy obraz (10-20)
```

### Zaawansowane opcje

Edytuj `train.sh` aby zmienić:
- `--max_train_steps=1500` - liczba kroków treningowych
- `--network_dim=16` - wymiar LoRA (16, 32, 64...)
- `--save_every_n_steps=250` - częstotliwość zapisywania checkpointów

## 📊 Monitorowanie treningu

### TensorBoard

```bash
cd sd-scripts
source ./venv/bin/activate
tensorboard --logdir=workspace/logs
```

Otwórz przeglądarkę: `http://localhost:6006`

## 🎁 Wyniki

Wytrenowane modele znajdziesz w:
```
sd-scripts/workspace/output/chroma_loras/
```

Pliki:
- `chroma_lora-000250.safetensors` - checkpoint po 250 krokach
- `chroma_lora-000500.safetensors` - checkpoint po 500 krokach
- `chroma_lora.safetensors` - finalny model

## ❓ FAQ

### Q: Instalacja trwa bardzo długo
**A:** Pobieranie modeli (~15GB) może zająć czas. Skrypt pokazuje postęp pobierania.

### Q: Błąd CUDA out of memory
**A:** Zmniejsz `batch_size` w `lora_config.toml` z `2` na `1`.

### Q: Jak użyć wytrenowanego modelu?
**A:** Skopiuj plik `.safetensors` do swojego UI (ComfyUI, Automatic1111) w folder `models/loras/`.

### Q: Ile zdjęć potrzebuję?
**A:** Minimum 10-15, optymalnie 20-50 zdjęć z dobrymi opisami.

### Q: Jak długo trwa trening?
**A:** ~15-30 minut dla 1500 kroków na RTX 4090 (zależy od datasetu).

## 🔄 Aktualizacja

```bash
cd sd-scripts
git pull
pip install -r requirements.txt --upgrade
```

## 📝 Licencja

Skrypty: MIT License  
sd-scripts: Zobacz [kohya-ss/sd-scripts](https://github.com/kohya-ss/sd-scripts)

## 🙏 Podziękowania

- [kohya-ss](https://github.com/kohya-ss) - sd-scripts
- [lodestones](https://huggingface.co/lodestones) - Chroma1-HD model

## 💬 Wsparcie

Issues: https://github.com/r0wer/autotrainchro/issues

---

**Autor**: r0wer  
**Wersja**: 1.0  
**Data**: 2024
