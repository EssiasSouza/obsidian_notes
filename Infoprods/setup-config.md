# Configuração de Gravação de Vídeos

Este documento reúne as configurações atuais utilizadas para gravação de aulas e vídeos. O objetivo é manter um padrão consistente de imagem e áudio entre as gravações.

---

# Webcam

## Equipamento

- Webcam: Logitech Brio
- Software: Logi Tune

## Campo de Visão (FoV)

| Configuração | Valor |
|--------------|-------|
| FoV | 78° |

---

## Ajustes de Imagem

| Configuração                                     | Valor                              |
| ------------------------------------------------ | ---------------------------------- |
| HDR                                              | On                                 |
| Anti-Flicker                                     | NTSC 60 Hz                         |
| Auto Focus                                       | Off                                |
| Focus Manual                                     | Ajustado para a distância do rosto |
| -----------/------------------------------------ |                                    |
| Auto Exposure                                    | Off                                |
| Exposure                                         | -5                                 |
| Gain                                             | 35                                 |
| Auto White Balance                               | Off                                |
| White Balance                                    | 4610 K                             |
| Brightness                                       | 128                                |
| Contrast                                         | 128                                |
| Saturation                                       | 128                                |
| Sharpness                                        | 161                                |
| Filter                                           | Original                           |

---

## Observações

- Utilizar foco manual para evitar mudanças de foco durante a gravação.
- O foco deve ser reajustado caso a posição da câmera seja alterada.
- Manter o White Balance fixo em 4610 K para garantir consistência entre diferentes gravações.
- O Gain foi reduzido para minimizar a redução de ruído aplicada pela câmera, preservando mais detalhes da imagem.
- O Sharpness foi aumentado para recuperar detalhes da pele, barba e cabelo sem utilizar filtros de embelezamento.

---

# OBS Studio

## Áudio

### Fonte

- Interface de áudio externa
- Microfone condensador BM-800

### Filtros (na ordem)

| Ordem | Filtro     | Configuração       |
| ----- | ---------- | ------------------ |
| 2     | Compressor | Threshold: -18 dB  |
|       |            | Ratio: 3:1         |
|       |            | Attack: 6 ms       |
|       |            | Release: 60 ms     |
|       |            | Output Gain: +2 dB |
| 3     | Gain       | +6 dB              |
| 4     | Limiter    | Threshold: -1 dB   |

---

## Configuração esperada do nível de áudio

Durante a gravação:

- Voz normal entre **-12 dB e -6 dB**
- Nunca atingir **0 dB** (clipping)

---

# Checklist antes de gravar

## Câmera

- [ ] Logi Tune aberto
- [ ] FoV em 78°
- [ ] HDR habilitado
- [ ] Exposure em -5
- [ ] Gain em 35
- [ ] White Balance em 4610 K
- [ ] Foco manual conferido
- [ ] Sharpness em 161

## OBS

- [ ] Microfone correto selecionado
- [ ] Compressor ativo
- [ ] Gain em +6 dB
- [ ] Limiter ativo
- [ ] Medidor de áudio oscilando entre -12 dB e -6 dB

---

# Melhorias futuras

## Iluminação

Priorizar melhorias na iluminação antes de alterar qualquer configuração da câmera. Uma iluminação frontal e difusa permite reduzir ainda mais o Gain da webcam, preservando detalhes da imagem e diminuindo a necessidade de processamento interno.

## Qualidade da captura

Verificar se:

- A Logitech Brio está capturando em 4K ou 1080p conforme desejado.
- O OBS está utilizando a resolução e o FPS corretos.
- Não há filtros adicionais aplicados no OBS para a webcam.
