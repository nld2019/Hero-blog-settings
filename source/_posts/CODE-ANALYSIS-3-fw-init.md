---
title: 'CODE-ANALYSIS:3-fw-init'
date: 2022-06-12 00:23:20
tags: code analysis, Wi-Fi, Driver;
---
Fri Jun  3 18:26:11 CST 2022

// init firmware

mlan_status
woal_init_fw(moal_handle *handle)

log the fw request time to req_fw_time

execute fw init, this function download FW using helper
	ret = woal_request_fw(handle);

// the real function used to download firmware
ret = woal_request_fw_dpc(handle,

// Download and init firmware DPC
woal_init_fw_dpc(moal_handle *handle)


// download firmware
ret = mlan_dnld_fw(handle->pmlan_adapter, &fw);
   ret = pmadapter->ops.dnld_fw(pmadapter, pmfw);
    
	// This function download firmware to card
    static mlan_status
	wlan_sdio_dnld_fw(pmlan_adapter pmadapter, pmlan_fw_image pmfw)


// request dpd data

// request txpwr data

// request cal data

// init data to MLAN
	ret = mlan_set_init_param(handle->pmlan_adapter, &param);

// this function init firmware
	ret = mlan_init_fw(handle->pmlan_adapter);
	/* Initialize firmware, may return PENDING */
		ret = wlan_init_fw(pmadapter);
		
			/* Initialize adapter structure */
				wlan_init_adapter(pmadapter);

		    wlan_adapter_get_hw_spec(pmadapter);


// send the first command 0xa9  to firmware
	ret = wlan_prepare_cmd(priv, HostCmd_CMD_FUNC_INIT, HostCmd_ACT_GEN_SET,
				       0, MNULL, MNULL);
/** Host Command ID : Function initialization */
#define HostCmd_CMD_FUNC_INIT 0x00a9

//prepare the command

// insert the command to queue to firmware

// process DPD data

// process txpwr data

// preapre command to firmware
/** Host Command ID : Cal data dnld */
#define HostCmd_CMD_CFG_DATA 0x008f


/** use to query chan region cfg setting in firmware */
#define HostCmd_CMD_CHAN_REGION_CFG 0x0242


/** Host Command ID : Get hardware specifications */
#define HostCmd_CMD_GET_HW_SPEC 0x0003


// Add interface DPC
woal_add_card_dpc(moal_handle *handle)
		/* Add interfaces */
		for (i = 0; i < handle->drv_mode.intf_num; i++) {
				if (handle->drv_mode.bss_attr[i].bss_virtual)
						continue;
				if (!woal_add_interface(handle, handle->priv_num,
										handle->drv_mode.bss_attr[i].
										bss_type)) {
						ret = MLAN_STATUS_FAILURE;
						goto err;
				}
		}

// print driver version number
	woal_get_version(handle, str_buf, sizeof(str_buf) - 1);
		PRINTM(MMSG, "wlan: version = %s\n", str_buf);

// This function add new  interface.
moal_private *
woal_add_interface(moal_handle *handle, t_u8 bss_index, t_u8 bss_type)

// define interface name, such as mlan0, wlan0, uap0 etc.

// init bss type, bss role type


// init tcp session queue

// init tx status queue


woal_init_sta_dev(dev, priv);


/* Register cfg80211 for STA or Wifi direct */
if (woal_register_sta_cfg80211(dev, bss_type)) {


// send domain info command to fw
		priv->phandle->band = IEEE80211_BAND_2GHZ;
		woal_send_domain_info_cmd_fw(priv, MOAL_IOCTL_WAIT);
		priv->phandle->band = IEEE80211_BAND_5GHZ;
		woal_send_domain_info_cmd_fw(priv, MOAL_IOCTL_WAIT);
		priv->phandle->band = band;


// create sche scan work queue

// create csa work queue


