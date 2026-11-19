# Medtrum EasyView — Home Assistant Integration

🇬🇧 English | 🇮🇹 [Italiano](README_IT.md) | 🇷🇺 [Русский](README_RU.md)
---

## ❤️ Support the project

If you find this integration useful, you can support its development:

[![Boosty](https://img.shields.io/badge/Boosty-Поддержать-F15F2C?style=for-the-badge&logo=boosty&logoColor=white)](https://boosty.to/alex_khmelenko)

Every donation helps to develop the integration faster: new features, bug fixes, and support for insulin pumps (planned).

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=outsiderz&repository=easyview-easyfollow&category=integration)

![HACS Custom](https://img.shields.io/badge/HACS-Custom-41BDF5?style=flat-square)
![GitHub Release](https://img.shields.io/github/v/release/outsiderz/easyview-easyfollow?style=flat-square)
![GitHub License](https://img.shields.io/github/license/outsiderz/easyview-easyfollow?style=flat-square)

This integration allows you to display real-time CGM (Continuous Glucose Monitor) data from **Medtrum** sensors in Home Assistant, read via the **EasyView** cloud.

---

## How it works

The Medtrum CGM sensor transmits data to the **EasyView** app on the patient's phone. Via the Follow sharing feature, this data is sent to the cloud and made available to a second account through the **EasyView Follow** function.

The Home Assistant integration connects to this Follow account to read the data in real time.

---

## Prerequisites

- An active Medtrum CGM sensor
- The **EasyView** app installed on the patient's phone, with a registered Medtrum account
- A **second Medtrum account** to use as a Follow account (can be a secondary account of yours)
- Home Assistant with HACS installed

---

## Step 1 — Invite the Follow account from EasyView

On the patient's phone, in the **EasyView** app:

1. Open the app and go to **Settings**
2. Select **Follow** or **Sharing**
3. Enter the email address of the second account
4. Send the invitation

---

## Step 2 — Accept the invitation and verify access

On the device where you received the invitation:

1. Accept the invitation via the received email or notification
2. Log in to the EasyView Follow account
3. Verify that the patient's glucose data is visible

---

## Step 3 — Install the integration in Home Assistant

### Via HACS

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=outsiderz&repository=easyview-easyfollow&category=integration)

1. Click the button above, or open HACS in Home Assistant manually
2. Go to **Integrations** → click the three dots in the top right → **Custom repositories**
3. Enter the repository URL: `https://github.com/outsiderz/easyview-easyfollow`
4. Select the category **Integration**
5. Click **Add**
6. Search for **EasyView** in HACS and install it
7. Restart Home Assistant

### Manual installation

1. Copy the `custom_components/easyview` folder to your Home Assistant `custom_components` directory
2. Restart Home Assistant

---

## Step 4 — Configure the integration

1. In Home Assistant go to **Settings** → **Devices & services**
2. Click **Add integration** and search for **EasyView**
3. **Step 1 — Credentials:**
   - **Email** and **Password** of the EasyView Follow account
   - **Unit of measurement** — `mg/dL` or `mmol/L`
4. **Step 2 — Thresholds:**
   - **High glucose threshold** — value above which the *Is High* sensor activates (default: 180 mg/dL or 10.0 mmol/L)
   - **Low glucose threshold** — value below which the *Is Low* sensor activates (default: 70 mg/dL or 3.9 mmol/L)
   
   Thresholds are entered in the unit chosen in step 1.
5. Click **Submit**

If the credentials are correct, the integration will connect and the sensors will be created automatically.

---

## Available sensors

For each monitored user, the following are created:

| Sensor | Description | Unit |
|--------|-------------|------|
| Glucose | Current value | mg/dL or mmol/L |
| Glucose Trend | Text + icon (Stable, Increasing, Decreasing, etc.) | — |
| Sensor Status | Normal / Warming up / Needs calibration | — |
| Battery | Sensor battery percentage | % |
| Minutes since update | Minutes since the last sensor reading | min |
| Calibration due | Measurements remaining until the next calibration | — |
| Sensor lifetime | Total sensor runtime | min |
| Is High | On / Off (configurable threshold) | — |
| Is Low | On / Off (configurable threshold) | — |

---

## Dashboard card

To display the reading on a dashboard, [SugarTV Card](https://github.com/wiltodelta/homeassistant-sugartv-card) (HACS) works with this integration out of the box. Select the glucose sensor, and it picks up `Glucose Trend` and `Minutes since update` automatically:

```yaml
type: custom:sugartv-card
glucose_value: sensor.patient_glucose
```

![SugarTV Card in a wide and a tall dashboard slot](https://raw.githubusercontent.com/wiltodelta/homeassistant-sugartv-card/v0.22.0/sugartv-card-layouts.png)

---

## Modifying alert thresholds

Thresholds can be changed at any time without reinstalling:

1. In Home Assistant go to **Settings** → **Devices & services**
2. Find the **EasyView** integration and click the ✏️ **Configure** icon
3. Update the **High glucose threshold** and/or **Low glucose threshold**
4. Click **Submit**

Thresholds are displayed in the unit you chose during setup (mg/dL or mmol/L).

---

## Re-authentication

If the integration stops working due to an expired session:

1. In Home Assistant go to **Settings** → **Devices & services**
2. You will see a notification on the EasyView integration — click **Re-authenticate**
3. Re-enter your credentials

There is no need to remove and reinstall the integration.

---

## Localization

The integration is fully localized in:

- 🇬🇧 English
- 🇮🇹 Italiano
- 🇷🇺 Русский

Sensor names **and state values** (trend, status) are translated automatically based on the Home Assistant language.

---

---

## ❤️ Support the project

If you find this integration useful, you can support its development:

[![Boosty](https://img.shields.io/badge/Boosty-Поддержать-F15F2C?style=for-the-badge&logo=boosty&logoColor=white)](https://boosty.to/alex_khmelenko)

Every donation helps to develop the integration faster: new features, bug fixes, and support for insulin pumps (planned).

## Common issues

| Issue | Solution |
|-------|----------|
| Authentication error | Check the email and password of the Follow account |
| No data visible | Verify that the invitation has been accepted and data is visible in the Follow account |
| Sensors unavailable | Check internet connectivity and restart the integration |
| Battery shows wrong value | Update to the latest version of the integration |

---

## Credits

**Current maintainer:** [@outsiderz](https://github.com/outsiderz)

This integration was originally based on the [dmsmartech/easyview](https://github.com/dmsmartech/easyview) project (MIT License). Since then, it has been substantially rewritten:

- 🩸 Rewritten API client for Medtrum EasyView Follow accounts
- 🌍 Full localization (English, Russian, Italian)
- 🎛️ Two-step config flow with unit-aware thresholds (mg/dL / mmol/L)
- 📊 Added new sensors (calibration due, sensor lifetime)
- 🎨 Local brand icons for Home Assistant 2026.3+
- 🔧 Fixed critical bugs (config_entry, session leaks, dt_util)

**License:** MIT — see [LICENSE.txt](LICENSE.txt)

API usage is based on the open source project [nl-ruud/nightscout-easyview](https://github.com/nl-ruud/nightscout-easyview).

Special thanks to the Home Assistant community for testing and feedback.