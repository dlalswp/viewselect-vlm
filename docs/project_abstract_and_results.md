# Final Project Wrap-up

## Project Title
Budget-Aware Active Multi-View Scene Understanding

## One-line Summary
멀티뷰 장면 이해에서 다음 시점을 선택하고 언제 촬영을 멈출지 결정하는 active perception prototype을 구현하고, 외부 실사 멀티뷰 보강으로 성능을 개선한 프로젝트.

## Problem
제한된 view budget 안에서 장면 이해 품질을 유지하면서, 추가로 어떤 시점을 봐야 하는지와 언제 충분하다고 판단할지를 동시에 결정하는 시스템이 필요했다.

## What I Built
- front 기준 partial state space 설계
- teacher action / stop label 생성
- selector(next-view) + stop(binary) 모델 학습
- interactive guidance prototype 구현
- ARKitScenes raw 기반 external pseudo 5-view 데이터 보강
- augmented retrain 및 rollout/guidance 재평가

## Key Results
- Final avg_final_views_per_scene: 4.5
- Final mean_step_nextview_agreement: 0.53125
- Final mean_step_stop_agreement: 1.0
- Guidance final observed views: ['front', 'left', 'front_left', 'right', 'front_right']
- Guidance termination: no_remaining_candidates

## Data Summary
- Total images: 183
- Total scenes: 45
- Train scenes: 16
- Val scenes: 8
- Demo scenes: 21
- External caption rows: 80
- External scenes captioned: 16

## Best Models
- Selector best: hist_gb
- Selector val_macro_f1: 0.5609989156953104
- Selector val_top2_accuracy: 0.8695652173913043
- Stop best: hist_gb
- Stop best threshold: 0.2
- Stop val_macro_f1: 1.0

## Files
- comparison_table: /content/drive/MyDrive/grad_project_master/03_results/tables/41_final_experiment_comparison.csv
- artifact_index: /content/drive/MyDrive/grad_project_master/03_results/tables/41_artifact_index.csv
- portfolio_summary_json: /content/drive/MyDrive/grad_project_master/03_results/tables/41_portfolio_summary.json
