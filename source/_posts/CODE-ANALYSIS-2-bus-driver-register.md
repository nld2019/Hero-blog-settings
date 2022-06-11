---
title: 'CODE-ANALYSIS:2-bus-driver-register'
date: 2022-06-12 00:00:02
tags: code analysis, Wi-Fi, Driver;
---

Jun  3 16:36:02 CST 2022

//while Wi-Fi card is detected by the Wi-Fi driver. Will trigger the register work.

static void
woal_bus_register(struct work_struct *work)


register wlan_sdio operation struct to kernel
/* SDIO Driver Registration */
if (sdio_register_driver(&wlan_sdio)) {

	static struct sdio_driver REFDATA wlan_sdio = {
		.name = "wlan_sdio",
		.id_table = wlan_ids,
		.probe = woal_sdio_probe,
		.remove = woal_sdio_remove,

if the Wi-Fi sdio card is detected by the OS, .probe function will be invoked.

show the vendor info of the detected card.

update the card current card type, current is 8997.

// add the card to system via the above card type.
woal_add_card(card, &card->func->dev, &sdiommc_ops, card_type)) {

	static moal_if_ops sdiommc_ops = {
		.register_dev = woal_sdiommc_register_dev,
		.unregister_dev = woal_sdiommc_unregister_dev,
		.read_reg = woal_sdiommc_read_reg,


