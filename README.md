# Blox-fruits#
pygame.draw.rect(screen, (80, 200, 80) if z["stun"] <= 0 else (150, 150, 150), z["rect"])
            pygame.draw.rect(screen, (255, 0, 0), (z["rect"].x, z["rect"].y - 10, 35, 5))
            pygame.draw.rect(screen, (0, 255, 0), (z["rect"].x, z["rect"].y - 10, 35 * (z["hp"]/z["max_hp"]), 5))
        
        # Эффекты
        for eff in effects:
            if eff["type"] == "slash": pygame.draw.arc(screen, (255, 255, 255), (eff["x"]-20, eff["y"]-20, 40, 40), 0, 3.14, 3)

        # Игрок
        if PLAYER_IMG: screen.blit(PLAYER_IMG, (player.x, player.y))
        else: pygame.draw.rect(screen, (60, 140, 255), player)

        # UI - Статы
        pygame.draw.rect(screen, (100, 0, 0), (20, 20, 200, 20))
        pygame.draw.rect(screen, (255, 0, 0), (20, 20, 200 * (hp/max_hp), 20))
        draw_text(f"❤️ {int(hp)}", 230, 20, (255,255,255), 20)
        
        pygame.draw.rect(screen, (100, 50, 0), (20, 50, 200, 15))
        pygame.draw.rect(screen, (255, 150, 0), (20, 50, 200 * (hunger/max_hunger), 15))
        draw_text("🍗", 5, 45, (255,255,255), 20)
        
        pygame.draw.rect(screen, (0, 50, 100), (20, 75, 200, 15))
        pygame.draw.rect(screen, (0, 150, 255), (20, 75, 200 * (thirst/max_thirst), 15))
        draw_text("💧", 5, 70, (255,255,255), 20)
        
        draw_text(f"🪵 {wood}  🪨 {stone}  🧱 {blocks}", 250, 20, (255, 255, 255), 24)

        # UI - Инструменты
        start_x = W//2 - (len(tools) * 65) // 2
        for i, tool in enumerate(tools):
            rect = pygame.Rect(start_x + i*65, H-70, 60, 60)
            is_selected = (i == current_tool_index)
            color = (200, 200, 200) if is_selected else (80, 80, 80)
            pygame.draw.rect(screen, color, rect, border_radius=8)
            draw_text(tool, rect.centerx, rect.centery - 5, (0, 0, 0) if is_selected else (200, 200, 200), 14, center=True)

        # UI - Фрукт и Рюкзак
        if owned_fruit:
            data = fruits_data[owned_fruit]
            pygame.draw.rect(screen, data["color"], (20, H-80, 60, 60), border_radius=10)
            draw_text(data["name"], 30, H-60, (0,0,0), 16)
        pygame.draw.rect(screen, (150, 100, 50), (90, H-80, 60, 60), border_radius=10)
        draw_text("🎒", 100, H-70, (255,255,255), 30)

        if pickup_timer > 0: draw_text(pickup_message, W//2, 130, (255, 255, 0), 30, center=True)

        # UI - Рюкзак
        if backpack_open:
            pygame.draw.rect(screen, (30, 30, 40), (W//2 - 200, H//2 - 200, 400, 400), border_radius=15)
            draw_text("РЮКЗАК", W//2, H//2 - 180, (255, 255, 255), 30, center=True)
            draw_text(f"Блоки для стройки: {blocks} (Клавиша 6)", W//2, H//2 - 130, (255, 255, 255), 20, center=True)
            draw_text(f"Шлем: {armor['helmet'] or 'Нет'}", W//2, H//2 - 90, (200, 200, 200), 20, center=True)
            draw_text(f"Нагрудник: {armor['chest'] or 'Нет'}", W//2, H//2 - 60, (200, 200, 200), 20, center=True)
            draw_text("X", W//2 + 160, H//2 - 190, (255, 100, 100), 30)

        # UI - Меню фрукта
        if menu_open and owned_fruit:
            data = fruits_data[owned_fruit]
            pygame.draw.rect(screen, (20,20,30), (W-280, 40, 260, 300), border_radius=10)
            draw_text(data["name"], W-260, 60, data["color"], 24)
            for i, ab in enumerate(data["abilities"]):
                btn = pygame.Rect(W-260, 120 + i*70, 230, 55)
                pygame.draw.rect(screen, (60,60,80), btn, border_radius=8)
                draw_text(ab, btn.x+10, btn.y+15, (255,255,255), 16)

    pygame.display.flip()

pygame.quit()
sys.exit()
