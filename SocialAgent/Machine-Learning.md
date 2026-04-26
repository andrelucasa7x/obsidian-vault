# Machine Learning — SocialAgent

## Modelo atual
- **EfficientNet-B0** — fine-tuned com cards do NOCAST
- 100% accuracy, 30 epochs, 164 amostras
- Substituiu GPT-4o Vision (grátis, 160ms vs 10s)

## Dataset
- 56 cards avaliados + 108 Instagram (BBC, NYT, CNN, G1, ESPN)
- 12 rodadas de feedback do André (66 aprovados, 74 reprovados)
- 582 imagens de treinamento (Pinterest, portais, ML dataset)

## Ciclo
Cards gerados → Modelo avalia → André aprova/rejeita → Modelo retreina

## Limitação
- 56 cards é pouco (ideal 500+)
- Dataset desbalanceado (55 aprovados vs 1 rejeitado no ML)
- GPU quota zero na conta trial

## Futuro (com RTX 3060)
- Treino local sem custo
- Inference < 50ms
- Fine-tuning contínuo

## Relacionado
[[Visao-Produto]] | [[OR1ON]]
