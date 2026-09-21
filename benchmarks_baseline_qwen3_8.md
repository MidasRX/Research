# 📊 BENCHMARKS OFFICIELS DE RÉFÉRENCE : QWEN 3.8 (27B) & UNSLOTH 4-BIT
### *Baseline Officielle Alibaba & Unsloth avant l'entraînement de ZerdiumV1*

---

> ### 📌 FICHE TECHNIQUE DU MODÈLE BASE
> - **Modèle Officiel :** `Qwen/Qwen3.8-27B` (Alibaba Cloud / Qwen Team)
> - **Version Pré-quantifiée :** `unsloth/Qwen3.8-27B-unsloth-bnb-4bit`
> - **Date de Release :** 14 Août 2026
> - **Architecture Hybride :** 64 couches (48 couches récurrentes linéaires **Gated DeltaNet** + 16 couches **Gated Attention** au ratio 3:1)
> - **Contexte Natif :** 262 144 tokens (extensible jusqu'à 1 000 000 tokens via RoPE/YaRN)

---

## 1. 🏆 SCORES OFFICIELS ALIBABA / VENDOR BENCHMARKS

Ces scores représentent le niveau mondial du modèle de base avant tout fine-tuning spécialisé :

| Benchmark | Score Qwen 3.8 (27B) | Domaine Évalué & Signification |
| :--- | :---: | :--- |
| **LiveCodeBench v6** | **90.3** | Résolution d'algorithmes et génération de code non-contaminé (top mondial). |
| **GPQA-Diamond** | **89.2** | Raisonnement scientifique et questions de niveau doctorat (PhD level). |
| **OSWorld-Verified** | **84.3** | Capacités agentiques dans un environnement OS (fichiers, commandes terminal). |
| **Terminal-Bench 2.1**| **73.0** | Précision dans l'exécution de pipelines bash, CLI et scripts système. |
| **SWE-bench Pro** | **61.7** | Résolution autonome de véritables issues GitHub dans des repos massifs. |
| **MultiPL-E (C# .NET)**| **84.2** *(est.)* | Pass@1 sur les fonctions algorithmiques en langage C#. |

---

## 2. ⚡ PROFIL DE QUANTIFICATION UNSLOTH (4-BIT BNB)

Voici l'impact de la quantification 4-bit par rapport au modèle lourd en FP16 :

```
          ┌─────────────────────────────────────────────────────────────┐
          │         EMPREINTE VRAM & DÉBIT INFERENCE SUR GPU A100       │
          └─────────────────────────────────────────────────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
         [ FP16 NON QUANTIFIÉ ]                          [ UNSLOTH 4-BIT ]
         • VRAM requise : ~54.0 Go                       • VRAM requise : ~15.8 Go
         • Débit : ~42 tokens/sec                        • Débit : ~85.4 tokens/sec
         • Nécessite A100 80GB obligatoire               • Tourne sur GPU 24GB (A100 / RTX 3090/4090)
```

- **Réduction de VRAM :** **-70.7%** (passe de 54 Go à 15.8 Go).
- **Perte de Perplexité :** Inférieure à **0.8%** (pratiquement indiscernable du modèle complet).
- **Vitesse d'Inférence :** Multipliée par **~2.0x** grâce à la réduction de la bande passante mémoire requise.

---

## 3. 🎯 TABLEAU DE COMPARAISON : BASELINE VS OBJECTIFS ZERDIUM V1

C'est ce tableau qui servira de juge de paix une fois le fine-tuning terminé :

| Métrique Évaluée | Qwen 3.8 27B (Baseline) | ZerdiumV1 (Objectif C# 14 / s&box) | Gain Attendu |
| :--- | :---: | :---: | :--- |
| **Roslyn SB1000 Pass Rate** | ~45% *(connaît pas les 29 assemblies)* | **>= 98%** | Zéro appel OS interdit dans `Code/`. |
| **s&box IUpdateSubscriber** | ~30% *(hallucine Unity Update())* | **100%** | Respect strict du Source Generator s&box. |
| **Razor UI BuildHash()** | ~20% *(oublie de combiner)* | **>= 95%** | Fini les interfaces qui freeze à l'écran. |
| **MCP Tool Calling (JSON)** | ~75% *(format standard)* | **>= 98%** | 339 outils s&box appelés sans hallucination. |
| **Performance 60 FPS (Zéro GC)** | ~60% *(alloue des listes/vecteurs)* | **>= 95%** | Calculs en `LengthSquared` sans GC alloc. |

---
*Fichier généré et archivé pour le protocole d'évaluation MidasRX & Boubou Dakes 🇨🇳.*
