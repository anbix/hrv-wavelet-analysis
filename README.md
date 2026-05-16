# HRV Wavelet Analysis

Вейвлет-анализ вариабельности сердечного ритма (ВСР) на Python.
В отличие от FFT, даёт **частотно-временное** представление: позволяет
отслеживать динамику мощности в HF / LF / VLF во времени.

[English version](README_EN.md)

## Что внутри

| Раздел | Что |
|---|---|
| 1 | Подготовка сигнала: кубическая интерполяция RR → 4 Гц |
| 2 | CWT (Морле): прямое преобразование, матрица W(a, b) |
| 3 | Скалограмма \|W(a, b)\|² с log-шкалой частоты |
| 4 | Динамика мощности: HF(t), LF(t), VLF(t) |
| 5 | Интегральные показатели: HF, LF, VLF, TP, %-нормировка, LF/HF |
| 6 | SDHF, SDLF, SDVLF, SDTP — изменчивость регуляторных контуров |
| 7 | Сравнение FFT vs Wavelet |
| 8 | Сравнение разных базисных вейвлетов (Морле, Гаусс) |

## Зачем это для wearable / Healbe

ВСР с браслета — **нестационарный** сигнал. Классический FFT усредняет всё в одну
оценку, теряя информацию о коротких событиях (стресс-эпизоды, экстрасистолы,
переходные процессы). CWT сохраняет временную локализацию: это критично для
непрерывного мониторинга функционального состояния.

Дополнительный бонус — **SDHF, SDLF, SDVLF** оценивают изменчивость регуляторных
контуров во времени, что невозможно получить из обычного FFT.

## Стек

NumPy, SciPy (`CubicSpline`), pandas, matplotlib, **PyWavelets** (`pywt`).

## Результаты на демо-данных

| Показатель | Wavelet | FFT |
|---|---|---|
| HF, % | 65.7 | 58.5 |
| LF, % | 33.7 | 31.6 |
| VLF, % | 0.6 | 9.9 |
| LF/HF | 0.51 | — |
| SDHF | 40.94 | — |
| SDLF | 20.34 | — |
| SDVLF | 0.38 | — |

![Скалограмма](figures/fig1_scaleogram.png)
![Динамика по полосам](figures/fig2_band_timeseries.png)
![FFT vs Wavelet](figures/fig3_fft_vs_wavelet.png)

## Как запустить

```bash
git clone https://github.com/<username>/hrv-wavelet-analysis.git
cd hrv-wavelet-analysis
pip install -r requirements.txt
jupyter notebook hrv_wavelet.ipynb
```

## Структура

```
hrv-wavelet-analysis/
├── hrv_wavelet.ipynb       # основной ноутбук
├── data/
│   └── rr_intervals.csv    # пример данных (тот же, что в hrv-analysis)
├── figures/                # графики, сохраняемые ноутбуком
├── requirements.txt
├── README.md / README_EN.md
└── LICENSE
```

## Связанные репозитории

- [hrv-analysis](https://github.com/anbix/hrv-analysis) — статистика, геометрия, FFT, ПАРС.
- hrv-nonlinear-dynamics — Херст, DFA (в работе).

## Литература

- Mallat S. *A Wavelet Tour of Signal Processing*. Academic Press, 1999.
- Akay M. *Time Frequency and Wavelets in Biomedical Signal Processing*. IEEE Press, 1998.
- Pichot V. et al. (1999). Wavelet transform to quantify HRV: a study in athletes. *Eur J Appl Physiol*.

## Автор

Анастасия Бирдина · birdinanastia@gmail.com

## Лицензия

MIT — см. [LICENSE](LICENSE).
