# [](#header-1)SimpleHeal Quellcode

[![zurück](https://lanehd.github.io/arrow-back-icon.png)](../ "Klicke hier, um zurückzugehen")

## [](#header-2)Warnung: Dieser Bereich ist nur für die gemacht, die sich damit auskennen!

ch.lanehd.heal.main.Main
```Java
package ch.lanehd.heal.main;

//IMPORTS
import org.bukkit.plugin.java.JavaPlugin;

import ch.lanehd.heal.commands.FeedCmd;
import ch.lanehd.heal.commands.HealCmd;
import ch.lanehd.heal.wip.SimpleHealCmd;
import de.stealthcoders.spu.SpigotPluginUpdater;

public class Main extends JavaPlugin {
	public void onEnable() {
		boolean heal = true;
		boolean feed = true;
		boolean autoupdate = true;
		boolean simpleheal = true;
		System.out.println("SimpleHeal v" + getDescription().getVersion() + " wurde erfolgreich geladen!");
		if (heal)
			getCommand("heal").setExecutor(new HealCmd());

		if (feed)
			getCommand("feed").setExecutor(new FeedCmd());

		if (simpleheal)
			getCommand("simpleheal").setExecutor(new SimpleHealCmd());
		SpigotPluginUpdater spu = new SpigotPluginUpdater(this/* Your Plugin */,
				"http://update.lanehd.bplaced.net/simpleheal.html");
		spu.enableOut(); // Enables an Output if there is a new Update and if the file was downloaded

		// spu.disableOut(); Disables the output
		if (spu.needsUpdate() && autoupdate) { // Check if there is an Update available
			spu.update(); // The Plugin will be automatically update
		} else if (autoupdate == false)
			System.out
					.println("SimpleHeal AutoUpdate ist deaktiviert! Bitte benutze /simpleheal update um zu updaten!");
		else
			System.out.println("SimpleHeal ist auf der neuesten Version!");
		// ...
		
		
		
	}
}
```

ch.lanehd.heal.commands.HealCmd
```Java
package ch.lanehd.heal.commands;

//IMPORTS
import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandExecutor;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class HealCmd implements CommandExecutor {

	@Override
	public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
		if (sender instanceof Player) {
			Player p = (Player) sender;
			if (args.length == 0) {
				if (p.hasPermission("SimpleHeal.heal.self") || p.hasPermission("SimpleHeal.heal.*")
						|| p.hasPermission("SimpleHeal.*")) {
					p.setHealth(20);
					p.sendMessage("§8[§bHeal§8] §aDu wurdest geheilt!");

				} else
					p.sendMessage("§8[§bHeal§8] §cDu hast nicht die benötigten Berechtigungen, um das zu tun!");
			}

			else if (args.length == 1) {
				if (p.hasPermission("SimpleHeal.heal.others") || p.hasPermission("SimpleHeal.heal.*")
						|| p.hasPermission("SimpleHeal.*")) {
					Player target = Bukkit.getPlayer(args[0]);
					if (target != null) {
						if (p == target) {
							p.setHealth(20);
							p.sendMessage("§8[§bHeal§8] §aDu wurdest geheilt!");

						} else {
							target.setHealth(20);
							target.sendMessage("§8[§bHeal§8] §aDu wurdest von §6" + p.getName() + "§a geheilt!");
							p.sendMessage("§8[§bHeal§8] §aDu hast §6" + target.getName() + "§a geheilt!");
						}

					} else {
						String t = args[0];
						p.sendMessage("§8[§bHeal§8] §cDer Spieler §6" + t + "§c ist nicht Online!");

					}
				} else
					p.sendMessage("§8[§bHeal§8] §cDu hast nicht die benötigten Berechtigungen, um das zu tun!");

			} else if (args.length >= 1)
				p.sendMessage("§8[§bHeal§8] §cBitte benutze /heal §6[Spieler]");

		} else if (args.length == 0)
			System.out.println("[Heal] Die Konsole hat keine Leben!");
		else if (args.length == 1) {
			Player target = Bukkit.getPlayer(args[0]);
			if (target != null) {
				target.setHealth(20);
				target.sendMessage("§8[§bHeal§8] §aDu wurdest von der Konsole geheilt!");
				System.out.println("[Heal] Du hast " + target.getName() + " geheilt!");
				
			} else {
				String t = args[0];
				System.out.println("[Heal] Der Spieler " + t + " ist nicht Online!");

			}

		} else if (args.length >= 1)
			System.out.println("[Heal] Bitte benutze /heal [Spieler]");
		return false;

	}

}
```

ch.lanehd.heal.commands.HealCmd
```Java
package ch.lanehd.heal.commands;

//IMPORTS
import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandExecutor;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class FeedCmd implements CommandExecutor {

	@Override
	public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
		if (sender instanceof Player) {
			Player p = (Player) sender;
			if (args.length == 0) {
				if (p.hasPermission("SimpleHeal.feed.self") || p.hasPermission("SimpleHeal.feed.*")
						|| p.hasPermission("SimpleHeal.*")) {
					p.setFoodLevel(20);
					p.sendMessage("§8[§bHeal§8] §aDu wurdest gefüttert!");

				} else
					p.sendMessage("§8[§bHeal§8] §cDu hast nicht die benötigten Berechtigungen, um das zu tun!");
			}

			else if (args.length == 1) {
				if (p.hasPermission("SimpleHeal.feed.others") || p.hasPermission("SimpleHeal.feed.*")
						|| p.hasPermission("SimpleHeal.*")) {
					Player target = Bukkit.getPlayer(args[0]);
					if (target != null) {
						if (p == target) {
							p.setFoodLevel(20);
							p.sendMessage("§8[§bHeal§8] §aDu wurdest gefüttert!");

						} else {
							target.setHealth(20);
							target.sendMessage("§8[§bHeal§8] §aDu wurdest von §6" + p.getName() + "§a gefüttert!");
							p.sendMessage("§8[§bHeal§8] §aDu hast §6" + target.getName() + "§a gefüttert!");

						}

					} else {
						String t = args[0];
						p.sendMessage("§8[§bHeal§8] §cDer Spieler §6" + t + "§c ist nicht Online!");

					}
				} else
					p.sendMessage("§8[§bHeal§8] §cDu hast nicht die benötigten Berechtigungen, um das zu tun!");

			} else if (args.length >= 1)
				p.sendMessage("§8[§bHeal§8] §cBitte benutze /feed §6[Spieler]");

		} else if (args.length == 0)
			System.out.println("[Heal] Die Konsole hat nie Hunger!");
		else if (args.length == 1) {
			Player target = Bukkit.getPlayer(args[0]);
			if (target != null) {
				target.setHealth(20);
				target.sendMessage("§8[§bHeal§8] §aDu wurdest von der Konsole gefüttert!");
				System.out.println("[§bHeal] Du hast " + target.getName() + " gefüttert!");

			} else {
				String t = args[0];
				System.out.println("[Heal] Der Spieler " + t + " ist nicht Online!");

			}

		} else if (args.length >= 1)
			System.out.println("[Heal] Bitte benutze /feed [Spieler]");
		return false;

	}

}
```

plugin.yml
```YAML
name: SimpleHeal
version: 1.2
main: ch.lanehd.heal.main.Main
author: LaneHD
description: Dies ist ein simples Heil-Plugin
commands:
   heal:
      description: Mit diesem Befehl kannst du einen Spieler heilen!
   feed:
      description: Mit diesem Befehl kannst du einen Spieler fuettern!
   simpleheal:
      description: Dieser Befehl ist fuer die Konfiguration des Plugins!
```
