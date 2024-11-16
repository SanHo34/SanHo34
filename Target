package com.example.trackerplus;

import org.bukkit.*;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerInteractEvent;
import org.bukkit.inventory.EquipmentSlot;
import org.bukkit.inventory.ItemStack;
import org.bukkit.plugin.java.JavaPlugin;

public class Main extends JavaPlugin implements Listener {
    private Player targetPlayer = null;

    @Override
    public void onEnable() {
        getServer().getPluginManager().registerEvents(this, this);
        getLogger().info("TrackerPlus enabled!");
    }

    @Override
    public void onDisable() {
        getLogger().info("TrackerPlus disabled!");
    }

    @EventHandler
    public void onPlayerInteract(PlayerInteractEvent event) {
        Player player = event.getPlayer();

        // Check for right-click and correct hand
        if (event.getHand() != EquipmentSlot.HAND) return;

        ItemStack item = player.getInventory().getItemInMainHand();

        // Compass interaction: Find closest player
        if (item.getType() == Material.COMPASS) {
            if (event.getAction().toString().contains("RIGHT_CLICK")) {
                Player closest = findClosestPlayer(player);
                if (closest != null) {
                    targetPlayer = closest;
                    player.sendMessage(ChatColor.GREEN + "Target set to: " + closest.getName());
                } else {
                    player.sendMessage(ChatColor.RED + "No players nearby.");
                }
            }
        }

        // Diamond interaction: Track target
        if (item.getType() == Material.DIAMOND) {
            if (event.getAction().toString().contains("RIGHT_CLICK")) {
                if (targetPlayer != null) {
                    if (player.getWorld().equals(targetPlayer.getWorld())) {
                        // Consume one diamond
                        item.setAmount(item.getAmount() - 1);

                        // Create firework effect
                        Location targetLocation = targetPlayer.getLocation();
                        Location playerLocation = player.getLocation();
                        Vector direction = targetLocation.toVector().subtract(playerLocation.toVector()).normalize();
                        Location fireworkLocation = playerLocation.add(direction.multiply(2));

                        player.getWorld().spawnParticle(Particle.FIREWORKS_SPARK, fireworkLocation, 10);
                        player.sendMessage(ChatColor.YELLOW + "Tracking " + targetPlayer.getName() + "...");
                    } else {
                        player.sendMessage(ChatColor.RED + "The target is not in the same world.");
                    }
                } else {
                    player.sendMessage(ChatColor.RED + "No target set. Use the compass first.");
                }
            }
        }
    }

    private Player findClosestPlayer(Player player) {
        double closestDistance = Double.MAX_VALUE;
        Player closestPlayer = null;

        for (Player other : player.getWorld().getPlayers()) {
            if (other.equals(player)) continue;

            double distance = player.getLocation().distance(other.getLocation());
            if (distance < closestDistance) {
                closestDistance = distance;
                closestPlayer = other;
            }
        }
        return closestPlayer;
    }
}
