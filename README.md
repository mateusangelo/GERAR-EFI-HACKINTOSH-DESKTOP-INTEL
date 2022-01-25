# Gerando pasta EFI e Instalando Hackintosh com OC Auxiliary Tools (OCAT)

## Pré-requisitos:

- Ter um Pendrive USB de preferencia 3.0
- Guia para Desktop Intel para EFI Bases de 1th até 11th

## 1) Definir Hardware:
Gerar report de Hardware do [Aida64](https://www.aida64.com) e Definir:

* Processador:
* Chipset Placa Mãe:
* Placa de Vídeo #1:
* Placa de Vídeo #2:
* Placa de Rede:

Extra: [Vídeo](https://youtu.be/9v3U72dPZTo) detalhado de como instalar e usar o Aida64

## 2) Verificar Placa de Vídeo
 Verificar se sua placa de Vídeo Externa (eGPU) é compatível com Hackintosh conforme lista abaixo. Caso voce só tenha iGPU ignorar essa parte (Apenas Nativas)

- Placas de Videos Compativeis com MacOS Monterey [https://youtu.be/Z_1Il-jQuq8](https://youtu.be/Z_1Il-jQuq8)

## 3) Definir EFI Template OC Auxiliary Tools

- Desktop 1th [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI%20Clarkdale%20e%20Lynnfield%201th.zip)
- Desktop 2th Sandy Bridge [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_2th_Sandy_Bridge.zip)
- Desktop 3th Ivy Bridge [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Ivy-Bridge_3th.zip)
- Desktop 4th Haswell [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Haswell_4th.zip)
- Desktop 6th SkyLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_SkyLake_6th.zip)
- Desktop 7th KabyLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_KabyLake_7th.zip)
- Desktop 8th CoffeeLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Coffee_Lake_8th.zip)
- Desktop 9th CoffeeLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Coffee_Lake_8th.zip)
- Desktop 10th CometLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Comet_Lake_10th.zip)
- Desktop 11th RocketLake [Download](https://github.com/mateusangelo/GERAR-EFI-HACKINTOSH-DESKTOP-INTEL/raw/main/EFI_Rocket_Lake_11th.zip)

* Para processadores F ou SMBIOS MacPro6,1, MacPro7,1 ou iMacPro1,1: Limpar todos os itens no Device Properties. 
                                                                                                                                                           
### 4.3) Boot Args
Em Nvram -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args, adcionar parametros opcionais conforme lista abaixo:

* Especificos para GPU

| Boot-arg       | Description                                                                                                                                                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| agdpmod=pikera | Disables Board-ID checks on AMD Navi GPUs (RX 5000 & 6000 series). Without this you'll get a black screen. Don't use on Navi Cards (i.e. Polaris and Vega).                                                                                        |
| \-igfxvesa     | Disables graphics acceleration in favor of software rendering. Useful if iGPU and dGPU are incompatible or if you are using an NVIDIA GeForce Card and the WebDrivers are outdated after updating macOS, so the display won't turn on during boot. |
| \-wegnoegpu    | Disables all GPUs but the integrated graphics on Intel CPU. Use if GPU is incompatible with macOS. Doesn't work all the time.                                                                                                                      |
| \-wegnoigpu    | to disable internal GPU                                                                                                                                                                                                                            |
| nvda\_drv=1    | Enable Web Drivers for NVIDIA Graphics Cards (supported up to macOS High Sierra only).                                                                                                                                                             |
| nv\_disable=1  | Disables NVIDIA GPUs (don't combine this with nvda\_drv=1)                                                                                                                                                                                         |

* Especificos para Soluções de problemas 

| Boot-arg                | Description                                                                                                                                                                                                                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-v                     | \_V\_erbose Mode. Replaces the progress bar with a terminal output with a bootlog which helps resolving issues. Combine with debug=0x100 and keepsyms=1                                                                                                                                                  |
| debug=0x100             | Disables macOS'es watchdog. Prevents the machine from restarting on a kernel panic. That way you can hopefully glean some useful info and follow the breadcrumbs to get past the issues.                                                                                                                 |
| keepsyms=1              | Companion setting to debug=0x100 that tells the OS to also print the symbols on a kernel panic. That can give some more helpful insight as to what's causing the panic itself.                                                                                                                           |
| dart=0                  | Disables VT-x/VT-d. Nowadays, DisableIOMapper Quirk is used instead.                                                                                                                                                                                                                                     |
| cpus=1                  | Limits the number of CPU cores to 1. Helpful in cases where macOS won't boot or install otherwise.                                                                                                                                                                                                       |
| npci=0x2000/npci=0x3000 | Disables PCI debugging related to kIOPCIConfiguratorPFM64. Alternatively, use npci=0x3000 which also disables debugging of gIOPCITunnelledKey. Required when stuck at PCI Start Configuration as there are IRQ conflicts related to your PCI lanes. Not needed if Above4GDecoding can be enabled in BIOS |
| \-no\_compat\_check     | Disables macOS compatibility check. For example, macOS 11.0 BigSur no longer supports iMac models introduced before 2014. Enabling this allows installing andd booting macOS on otherwise unsupported SMBIOS. Downside: you can't install system updates if this is enabled.                             |

* Especificos para Audio

| Boot-arg | Description                                                                                                                                                                                       |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| alcid=1  | Used for setting layout-id for AppleALC, see supported codecs (opens new window)to figure out which layout to use for your specific system. More info on this is covered in the Post-Install Page |


### 4.4) Gerando Serial
No OCAT em Plataform Info, clicar em Generate ao lado do campo SytemProductName e ROM

### 4.5) Gravar EFI
Salvar todos os ajustes feitos na EFI para adicionar no Pendrive de Boot

## 5) Criar Pendrive de Boot:
Apos definir a versão a ser instalada, no link abaixo e fazer download da Imagem do MacOS a ser instalado:

* [Download](https://www.olarila.com/topic/6278-olarila-vanilla-images/) ISO From Olarila

Links alternativos para Baixar o Big Sur 11.2

* [ISO Big Sur 11.2](https://bit.ly/ISOBigSUR3)
* [ISO Big Sur 11.2](https://bit.ly/ISOBigSUR2)
* [ISO Big Sur 11.2](https://bit.ly/ISOBigSUR1)

Apos baixar a imagem, usar o Rufus para gravar ela pendrive de boot.

Alternativo: Método oficial do Opencore para Criar Pendrive de Boot Direto da Apple no Windows [Aqui
](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#making-the-installer-in-windows)

## 6) Copiando EFI para Pendrive:
Abrir o OCAT, clicar em MountESP, selecionar o pendrive na lista de disco disponiveis e clicar em Mount.

Apos abrir a partição EFI, delete todo o conteudo da partição e copie a pasta EFI gerada no passo 4 para ela.


## 7) Pre-Install Ajuste de Bios:
* Disable
	* Fast Boot
	* Secure Boot
	* Serial/COM Port
	* Parallel Port
	* VT-d 
	* CSM
	* Intel SGX
	* Intel Platform Trust
	* CFG Lock 
	* Thunderbolt (For initial setup)
* Enable
	* VT-x
	* Above 4G decoding
		* 2020+ BIOS Notes: When enabling Above4G, Resizable BAR Support may become an available on some Z490 and newer motherboards. Please ensure this is Disabled instead of set to Auto.
	* Hyper-Threading
	* Execute Disable Bit
	* EHCI/XHCI Hand-off
	* OS type: Windows 8.1/10 UEFI Mode
	* DVMT Pre-Allocated(iGPU Memory):  
		* 32MB Até Ivy Bridge (03Gen) rodando Catalina, 
		* 64MB para Ivy Bridge com Big Sur, Haswell ou Superior.  
	* SATA Mode: AHCI

Extra: Exemplo Ajuste de Bios 👉 [Bios 01](https://youtu.be/FOrov-ur4qA) [Bios 02](https://youtu.be/EUolzdvEpDA)

## 8) Instalar Mac OS:
Fazer boot pelo pendrive, no menu do Opencore, selecionar Install Mac Os [Nome da versão que escolher], e ir até o final.

* Caso tenha problema com video integrado, colocar o atributo -igfxvesa e tentar novamente
* Caso apareça mensagem de proibido, apos selecionar imagem para instalar, trocar pendrive de porta, escolhendo uma 2.0/3.0 na traseira da maquina.

Extra: Como instalar MacOS [Big Sur](https://youtu.be/i7F7WtXXbhs) e [Monterey](https://youtu.be/b6g-W4gyQm0)

## 9) Post Install:
* Copiar a pasta EFI oara a partição EFI do disco que foi instalar o sistema, para dar boot sem pendrive.

Mapeament de USBs:

* Como Mapear Portas USBs até big sur 11.2 👉 👉 [https://youtu.be/X6djBycrcx4 ](https://youtu.be/b6g-W4gyQm0)
* Como Mapear Portas USBs acima big sur 11.3 👉 👉 [https://youtu.be/4z76yO5_2aU](https://youtu.be/b6g-W4gyQm0)
* Ajustando Audio HDMI e Mapeando Conectores 👉 👉 [https://youtu.be/NIoPq2w8q18](https://youtu.be/NIoPq2w8q18)
