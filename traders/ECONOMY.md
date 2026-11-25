# Економіка торговців (Dr.Jones Trader)

Торговці: **Benassi** (`TraderConfig.txt`, Новодмитрівськ), **Chereshnia** (`TraderConfig_Mogilevka.txt`, Могилівка),
**KONOXA**, чорний ринок (`TraderConfig_BlackMarket.txt`, Старий Яр).

## Ролі
| Торговець | Що продає | Кому вигідно продавати |
|---|---|---|
| Benassi | Базова військова екіпіровка, AK74, Glock, Mosin, патрони за базовою ціною | Зброя та патрони (40% «справедливої» ціни) |
| Chereshnia | Мисливська зброя, цивільний одяг, їжа, насіння | Тільки вона скуповує м'ясо, рибу, гриби та продукти. Зброю та патрони бере за 80% від цін колонії |
| KONOXA (ЧР) | **Ексклюзив**: M4A1, Aug, SVD, ASVAL, M14, FAMAS, AK101, UMP, AKM_Gold, глушники, топові FOG-броня/шоломи/рюкзаки | Скуповує свою зброю за 45% (найкраща ціна, «скупщик краденого») |

## Правила цін
1. Є одна «справедлива» шкала цін зброї (Derringer 300 … SV98 6000), вона залежить від рідкості в types.xml.
2. **ЧР дорожчий за легальних торговців у 1.4 раза.** Ексклюзивні предмети ЧР прибрано з продажу у Benassi.
3. Викуп становить ~30% ціни продажу для спорядження, їжі та коробок патронів і 40% для зброї (45% на ЧР).
4. **Жодного арбітражу:** жоден торговець не купує предмет дорожче, ніж хтось інший його продає. Скрипт перевіряє це для всіх трьох торговців.
5. Один предмет коштує однаково в обох легальних торговців.
6. Патрони рахуються за штуку. Коробка коштує як 20 патронів мінус 5%; на ЧР коробка дорожча у 1.4 раза.

## Ключові ціни (продаж / викуп)
| | Benassi | Chereshnia | ЧР |
|---|---|---|---|
| AK74 / AK74_Green | 2500 / 1000 | — / 800 | 3500 / 1120 (Green) |
| M4A1 | — / 2000 | — / 1600 | 7000 / 2250 |
| SVD | — / 1800 | — / 1440 | 6300 / 2020 |
| FAMAS | — / 1400 | — / 1120 | 4900 / 1580 |
| Броня | PlateCarrierVest_Camo 2000 | — | CPC/DPC 2800, Osprey 3500, Gen4 4500 |
| Шолом | Mich2001 1000 | — | FAST/Caimen 2000 |
| Рюкзак | DrawBridge_MC 3000 | — | DrawBridge 4000, BlackJack 4500 |

## Технічні виправлення
* Класи, яких немає в types.xml, замінено: `Mosin9130_Woodland→Mosin9130_Green`, `UKAssVest_Desert→UKAssVest_Khaki`,
  `HuntingVest_Desert→HuntingVest`, `Mag_AK74_30Rnd_Green→Mag_AK74_30Rnd` (Benassi) / `Mag_AK74_45Rnd` (ЧР).
* У Benassi `AK74_Green` замінено на `AK74`, бо зелений тепер ексклюзив ЧР.
* ЧР: прибрано повтори (`FOG_Bag_DrawBridge_MCAL`, `FOG_Jacket_SoftShell_MC`, `FOG_Vest_CPC_AOR2`), кількість коробок патронів змінено з 20 на 1.
* Зі скупки прибрано дублі предметів, які вже є у продажу того ж торговця.

## Перевірити в грі (класів немає в types.xml, можливо, вони з модів)
`AKM_Gold`, `Mag_AKM_30Rnd_Gold`, `AK_WoodHndgrd_Gold`, `AK_WoodBttstck_Gold`, `Mag_CMAG_30Rnd_Green`, `M4_MPBttstck_Green`,
`M4_OEBttstck_Green`, `AK74_Hndgrd_Camo`, `FOG_Tactical_Hoodie_Black`, `FOG_MTF_Pants_FK`, `Trumpet`, `MakarovPB`, `ChestRigVest_Khaki`,
`FOG_Hoodie_Jacket_Rolled_ERDL`, `Lassi_Mint`, `SmallGlassBottle_Afza`, `MessTin`, `MortarMix`, `CigarettePack_Chernamorka`,
`AmmoBox_12gaBeanbag_10Rnd`, `Banana`, `Shrimp`, `Catfish`, `SaltyMix`, `Expansion_PlateCarrierVest_Yeet`.
