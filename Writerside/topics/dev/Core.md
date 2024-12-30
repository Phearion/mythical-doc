# 🫀 Core

## Project Structure Overview

### Core Components
```
src/
├── events/              # Event handlers and core game systems
├── SlashCommands/       # Command implementations
├── structures/          # Base classes and interfaces
├── utils/              # Helper functions and models
└── assets/             # Game resources and configurations
```

## Core Systems Architecture

### 1. Command System
The bot uses a single entry point through `/game` command, implemented in `SlashCommands/mythical/game.ts`.

```typescript
// SlashCommands/mythical/game.ts
export default new SlashCommand({
    name: 'myth',
    description: 'Open the menu game of Mythical',
    run: async ({ interaction }) => {
    
    }
}
```

### 2. Interaction System

#### Button Handler
Located in `events/guild/interactions/buttons/buttonHandler.ts`, manages all button interactions:

```typescript
export const handleButtonInteraction = async (button: ButtonInteraction) => {
    const buttonId = button.customId.split('-')[0];
    switch (buttonId) {
        case 'hunt':
            await handleHuntButton(button);
            break;
        // ... other handlers
    }
};
```

#### Menu Categories
As of now, the bot has the following menu categories:
<img alt="Menu Categories" src="../images/game-menu/categories.png" heigth="250"/>

### 3. Game Systems

#### Hatchery System


#### Battle System
Located in `events/guild/mythicalSys/battle/`

#### Cardinal System
Located in `events/guild/mythicalSys/cardinal/`
- Bond management
- Level progression
- Home interaction
- Aesthetic customization

#### Housing System
Located in `SlashCommands/mythical/housingSystem/`
```typescript
interface HouseConfig {
    type: HouseType;
    furniture: FurnitureItem[];
    visitors: string[];
    income: number;
    lastCollection: Date;
}
```

#### Hunt System
Located in `SlashCommands/mythical/miniGames/huntSystem/`
- Party management
- Reward distribution
- Progress tracking
- Luminal interactions

### 4. Data Models

#### Core Models (in `utils/models/`)
```typescript
// PlayerProfile.ts
interface PlayerProfile {
    userId: string;
    username: string;
    experience: number;
    level: number;
    inventory: InventoryItem[];
    achievements: Achievement[];
}

// Cardinal.ts
interface Cardinal {
    id: string;
    level: number;
    bondLevel: number;
    aesthetic: CardinalAesthetic;
    skills: CardinalSkill[];
}
```

### 5. Asset Management

#### Configuration Files (`assets/json/`)
- `achievements.json`: Achievement definitions'
- `battleConfig.json`: Battle system parameters
- `cardinal-bond.json`: Cardinal bonding mechanics
- `cardinalDesc.json`: Cardinal descriptions (Lore, Skins etc.)
- `creatures.json`: Creature definitions (Luminals names, base stats)
- `elyaProgress.json`: Elya progression system (chats, paths)
- `houseConfig.json`: Housing system settings
- `huntRewads.json`: Hunt rewards distribution
- `infiniteRealms.json`: PVE mode configurations for each Realm (stages)
- `luminalsDesc.json`: Luminals descriptions (Lore, Types, Food regime etc.)
- `news.json`: In-game News and updates with rewards.
- `profileColors.json`: Profile color customization options
- `realmsEnemiesCompositions.json`: Enemies compositions for each Realm

#### Image Assets
Organized in categories:
- All images NOT related to: Luminals/Cardinals are stored within the repository
- Luminals and Cardinals images are stored and served from a server

## Development Guidelines

### 1. Adding New Features

We do not add slash commands directly. Instead, we use the `/game` command as the entry point for all interactions.
Instead, we add new features as buttons or interactions within the game menu/submenus.

#### New Button Handler
```typescript
// events/guild/interactions/buttons/newFeature/featureButton.ts
export async function handleFeatureButton(
    interaction: ButtonInteraction,
    action: string,
    params: string[]
) {
    switch (action) {
        case "action1":
            // Handle action
            break;
        // More actions
    }
}
```

### 2. Event System Integration

#### Creating New Event Handler
```typescript
// events/client/newEvent/handler.ts
export default class NewEventHandler extends Event {
    constructor() {
        super({
            name: "eventName",
            once: false
        });
    }
    
    async execute(...args: any[]) {
        // Event handling logic
    }
}
```

### 3. Database Integration

#### Model Creation
```typescript
// utils/models/NewFeature.ts
import mongoose from "mongoose";

const NewFeatureSchema = new mongoose.Schema({
    userId: { type: String, required: true },
    featureData: Object,
    createdAt: { type: Date, default: Date.now }
});

export default mongoose.model("NewFeature", NewFeatureSchema);
```

## Testing Guidelines

### Unit Testing Structure
```typescript
describe("Feature Test", () => {
    beforeEach(() => {
        // Setup test environment
    });

    it("should handle basic interaction", async () => {
        // Test implementation
    });
});
```

### Common Test Cases
1. Button interaction flows
2. Battle system mechanics
3. Cardinal bond calculations
4. House income generation
5. Hunt reward distribution

## Error Handling

```typescript
try {
    await handleFeature(interaction);
} catch (error) {
    console.error(`Error in feature: ${error}`);
    await interaction.reply({
        content: "An error occurred while processing your request.",
        ephemeral: true
    });
}
```

## Deployment Considerations

### Environment Setup
```env
DISCORD_TOKEN=your_token
MONGODB_URI=your_mongodb_uri
DEBUG_MODE=true
```

### Production Checklist (to better handle or to do)
- Configure error logging
- Set up monitoring
- Enable rate limiting
- Configure backup systems
- Set appropriate timeouts