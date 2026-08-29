
Readme FligthVR:
# FlightVR BUILD 0.6.0

## 📋 Visão Geral

Sistema completo de simulação de voo para VR com foco em **pouso realista**, **dinâmica atmosférica avançada** e **IA de piloto automático**. Integração automática com Sketchfab para modelos 3D.

**Versão:** 0.6.0  
**Engine:** Godot 4.x  
**Linguagem:** GDScript

---

## 🎯 Recursos Implementados

### ✅ CORE - Sistema Atmosférico
- **atmospheric_system.gd** - Dinâmica climática realista
  - Densidade do ar variável por altitude
  - Taxa de esfriamento troposférico (-6.5K/km)
  - Cálculo de pressão dinâmica (Q)
  - Força de arrasto (Drag)

- **wind_field.gd** - Campo de vento turbulento
  - Vento base configurável
  - Turbulência Perlin 3D
  - Gradiente vertical de vento (wind shear)
  - Rajadas sísmicas

### ✅ AIRFIELD - Sistema de Pistas
- **runway_system.gd** - Gerenciamento de pistas
  - Múltiplas pistas (09L/27R)
  - Verificação de alinhamento (±5°)
  - Validação de glideslope (±1.5°)
  - Cálculo de zona segura de pouso

- **landing_aid_hud.gd** - HUD de assistência
  - Indicador ILS virtual
  - Velocidade vertical em tempo real
  - Status de alinhamento
  - Avisos de segurança

- **touchdown_physics.gd** - Física de impacto
  - Modelo de trem de pouso (spring-damper)
  - Cálculo de G-forces
  - Detecção de bounce
  - Avaliação de danos estruturais

### ✅ AI - Controlador de Voo
- **flight_controller_ai.gd** - Piloto automático
  - Padrão 3-2-1 de aproximação
  - Compensação de vento
  - Descida controlada
  - Ativação/desativação manual

### ✅ UTILS - Gerenciador de Modelos
- **model_manager.gd** - Integração Sketchfab
  - Download automático de modelos
  - Suporte a GLTF/GLB
  - Organização por categoria (aeronaves, cockpits, cenário)
  - Fila de download assíncrono

### ✅ MAIN - Orquestrador
- **main.gd** - Controller principal
  - Inicialização de todos os sistemas
  - Loop de atualização integrado
  - Gerenciamento de entrada
  - Relatório de pouso

---

## 📁 Estrutura do Projeto

```
FlightVR/
├── src/
│   ├── core/
│   │   ├── atmospheric_system.gd
│   │   └── wind_field.gd
│   ├── airfield/
│   │   ├── runway_system.gd
│   │   ├── landing_aid_hud.gd
│   │   └── touchdown_physics.gd
│   ├── ai/
│   │   └── flight_controller_ai.gd
│   ├── utils/
│   │   └── model_manager.gd
│   └── main.gd
├── scenes/
│   └── main.tscn
├── assets/
│   ├── models/
│   │   ├── aircraft/      (Sketchfab)
│   │   ├── cockpit/       (Sketchfab)
│   │   └── scenery/       (Sketchfab)
│   └── audio/
├── project.godot
├── README.md
├── SETUP.md
└── .gitignore
```

---

## 🚀 Como Usar

### 1. **Configurar Sketchfab API**

```gdscript
# Em model_manager.gd
@export var sketchfab_api_key: String = "sua_chave_aqui"
```

Obtenha sua chave em: https://sketchfab.com/settings/api

### 2. **Adicionar Modelos**

Encontre modelos em:
- Aeronaves: https://sketchfab.com/search?q=aeronaves&type=models
- Cockpits: https://sketchfab.com/search?q=cockpit&type=models

Adicione os IDs ao manifesto em `model_manager.gd`:

```gdscript
var manifest = {
    "aircraft": [
        {"name": "Cessna 172", "sketchfab_id": "XXXXX", "category": "general_aviation"}
    ]
}
```

### 3. **Executar o Projeto**

```bash
# No Godot Editor
1. Abra o projeto
2. Pressione F5 (Play)
3. Os modelos serão baixados automaticamente
```

### 4. **Controles de Teste**

| Tecla | Ação |
|-------|------|
| **A** | Ativar/Desativar Autopilot |
| **W** | Aumentar Throttle |
| **S** | Diminuir Throttle |
| **↑** | Pitch Up |
| **↓** | Pitch Down |

---

## 📊 Dados Físicos Utilizados

### Atmosfera Padrão ISA
- Densidade ao nível do mar: **1.225 kg/m³**
- Taxa de esfriamento: **-6.5 K/km**
- Temperatura base: **288.15 K (15°C)**

### Trem de Pouso
- Rigidez (stiffness): **50,000 N/m**
- Amortecimento (damping): **5,000 N·s/m**
- Limite de velocidade segura: **3.0 m/s**
- Coeficiente de atrito: **0.8**

### Limites de Pouso Seguro
- Velocidade vertical: < 3 m/s
- Velocidade horizontal: < 30 m/s
- Alinhamento: ±5° (heading)
- Glideslope: ±1.5° (pitch)

---

## 🔧 Configurações Avançadas

### Modificar Dinâmica do Vento

Em `wind_field.gd`:
```gdscript
@export var wind_base: Vector3 = Vector3(5, 0, 10)  # m/s
@export var turbulence_scale: float = 0.3
@export var altitude_gradient: float = 0.02
@export var gust_frequency: float = 0.1
```

### Ajustar Sensibilidade de Pouso

Em `touchdown_physics.gd`:
```gdscript
@export var gear_stiffness: float = 50000.0
@export var gear_damping: float = 5000.0
@export var max_vertical_velocity: float = 3.0
```

### Mudar Velocidade de Aproximação

Em `flight_controller_ai.gd`:
```gdscript
@export var approach_speed: float = 50.0  # m/s
@export var descent_rate: float = 2.0     # m/s
```

---

## 📦 Dependências

- **Godot 4.0+**
- **GDScript 2.0**
- Conexão com internet (para Sketchfab API)

---

## 🎮 Próximas Features (BUILD 0.7+)

- [ ] Suporte completo a VR (OpenXR)
- [ ] Modelos 3D de aeronaves reais (Sketchfab)
- [ ] Sistema de combustível e consumo
- [ ] Aviônica avançada (FMS, autopilot aprimorado)
- [ ] Múltiplos aeroportos
- [ ] Replay de voos
- [ ] Ranking de pouso

---

## 📝 Notas de Desenvolvimento

### BUILD 0.6 Changelog
✅ Sistema atmosférico com vento turbulento  
✅ Física de pouso realista com trem de pouso  
✅ IA de piloto automático com padrão 3-2-1  
✅ HUD de assistência com ILS virtual  
✅ Integração Sketchfab para modelos 3D  
✅ Gerenciador automático de assets  

### Próximos Passos
1. Testar com modelos GLB do Sketchfab
2. Otimizar rendering para VR
3. Adicionar suporte a joystick/gamepad
4. Implementar gravação de telemetria

---

## 👤 Autor

**Doug Spinheiro**  
GitHub: [@dougspinheiro](https://github.com/dougspinheiro)

---

## 📄 Licença

Projeto em desenvolvimento. Todos os direitos reservados.

---

**Última atualização:** 2026-08-28  
**Status:** Em desenvolvimento ativo 🚀
