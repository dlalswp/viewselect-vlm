# Budget-Aware Active Multi-View Scene Understanding

제한된 촬영 예산(view budget) 안에서 장면 이해 품질을 유지하면서,  
**다음에 어떤 시점을 더 봐야 하는지**와 **언제 관측을 멈춰도 되는지**를 동시에 결정하는  
**active perception prototype**을 구현한 프로젝트입니다.

이 프로젝트는 단순한 멀티뷰 캡셔닝이 아니라,  
부분 관측 상태에서 **next-view selection**과 **stop decision**을 함께 수행하는  
경량 멀티뷰 장면 이해 파이프라인을 목표로 합니다.

---

## 1. Project Goal

멀티뷰 환경에서는 모든 시점을 끝까지 보는 것이 항상 효율적이지 않습니다.  
실제 시스템에서는 다음과 같은 질문이 중요합니다.

- 현재까지 본 시점만으로 장면 이해가 충분한가?
- 추가 시점을 본다면 어떤 시점이 가장 정보 이득이 큰가?
- 제한된 관측 예산 안에서 효율적으로 장면을 이해할 수 있는가?

이 프로젝트에서는 위 문제를 해결하기 위해:

- **partial observation state space**
- **teacher action / stop label generation**
- **selector(next-view) model**
- **stop(binary) model**
- **guidance-style prototype**
- **external real-scene pseudo 5-view augmentation**

을 포함한 전체 파이프라인을 구축했습니다.

---

## 2. What I Built

### Core Pipeline
- `front` 기준의 부분 관측 state space 설계
- 관측된 시점 집합에 따라 상태별 feature 구성
- **teacher action**(다음 추천 시점) 및 **teacher stop label** 생성
- **selector(next-view)** 와 **stop(binary)** 이중 정책 학습
- 실제 사용 흐름을 가정한 **interactive guidance prototype** 구현

### Data Augmentation
- 내부 촬영 멀티뷰 데이터 외에
- **ARKitScenes raw** 기반 외부 실사 데이터를 활용해
- **pseudo 5-view scene**을 구성
- caption cache, feature bank, teacher/feature state를 전부 재구축한 뒤
- augmented retrain 및 reevaluation 수행

---

## 3. Main Contributions

- 멀티뷰 장면 이해에서 **“무엇을 더 볼 것인가”** 와 **“언제 멈출 것인가”** 를 동시에 다룸
- 단순 분류가 아니라 **budget-aware active inference** 문제로 정식화
- 내부 데이터만이 아니라 **외부 실사 멀티뷰 보강**까지 포함해 실험 수행
- selector / stop / rollout / guidance까지 이어지는 **end-to-end research prototype** 완성
- 포트폴리오/면접에서 설명 가능한 수준의 **실험 비교, 결과 정리, artifact packaging** 완료

---

## 4. Final Results

### Final Rollout Performance
- **avg_final_views_per_scene**: `4.5`
- **mean_step_nextview_agreement**: `0.53125`
- **mean_step_stop_agreement**: `1.0`

### Guidance Prototype
- final observed views: `['front', 'left', 'front_left', 'right', 'front_right']`
- termination reason: `no_remaining_candidates`

### Best Models
- **Selector best model**: `hist_gb`
- **Selector val_macro_f1**: `0.560999`
- **Selector val_top2_accuracy**: `0.869565`
- **Stop best model**: `hist_gb`
- **Stop best threshold**: `0.2`
- **Stop val_macro_f1**: `1.0`

---

## 5. Data Summary

- **Total images**: `183`
- **Total scenes**: `45`
- **Train scenes**: `16`
- **Val scenes**: `8`
- **Demo scenes**: `21`
- **External caption rows**: `80`
- **External scenes captioned**: `16`

---

## 6. Repository Structure

```text
budget_aware_active_multiview/
├─ README.md
├─ docs/
│  └─ project_abstract_and_results.md
├─ results/
│  ├─ final_experiment_comparison.csv
│  ├─ artifact_index.csv
│  ├─ portfolio_summary.json
│  ├─ split_summary.json
│  ├─ external_caption_summary.json
│  ├─ final_rollout_metrics.json
│  ├─ final_rollout_scene_summary.csv
│  ├─ final_guidance_summary.json
│  ├─ selector_model_comparison.csv
│  └─ stop_model_comparison.csv
├─ configs/
│  └─ augmented_feature_columns.json
└─ manifests/
   ├─ 42_package_manifest.csv
   └─ 42_repo_structure.json
```

---

## 7. Key Files

- `docs/project_abstract_and_results.md`  
  프로젝트 전체 요약 및 핵심 결과 정리

- `results/final_experiment_comparison.csv`  
  baseline / deployable / augmented 단계 비교표

- `results/portfolio_summary.json`  
  포트폴리오용 핵심 요약 정보

- `results/final_rollout_metrics.json`  
  최종 rollout 성능 요약

- `results/final_guidance_summary.json`  
  guidance prototype 최종 종료 결과

- `configs/augmented_feature_columns.json`  
  최종 feature bank에 사용된 feature 정의

---

## 8. Notes

- 이 저장소는 **포트폴리오 / 연구 소개용 lightweight package**입니다.
- 대용량 raw dataset 및 무거운 모델 바이너리는 GitHub에 직접 포함하지 않을 수 있습니다.
- 본 프로젝트의 핵심은 앱/서비스 배포가 아니라,  
  **멀티뷰 장면 이해에서 active view selection과 stop policy를 함께 다룬 연구형 프로토타입**이라는 점입니다.

---

## 9. Future Extensions

향후 확장 방향은 다음과 같습니다.

- 실시간 카메라 입력 기반 online guidance
- 더 다양한 실제 실내/실외 멀티뷰 데이터로 확장
- lightweight VLM 또는 stronger multimodal backbone 적용
- quality-aware / uncertainty-aware next-view policy로 확장
- immersive video / adaptive streaming 문제와의 연결

---

## 10. Project Status

**Implemented and packaged as a complete research prototype.**  
현재 버전은 포트폴리오, 대학원 컨택, 면접 설명용 프로젝트로 사용할 수 있도록 정리된 상태입니다.
