# 📄 Hướng Dẫn Cấu Hình PaperMC (Tiếng Việt)

Tài liệu giải thích chi tiết toàn bộ file cấu hình `paper-global.yml` và `paper-world.yml` của PaperMC, dành cho mọi cấp độ quản trị server.

> **Phiên bản Paper:** 1.21 trở lên  
> **Áp dụng cho:** Tất cả loại server — Survival, Farm, Minigame, PvP

---

## 📁 Cấu Trúc File

```
server/
├── config/
│   ├── paper-global.yml            ← Ảnh hưởng toàn bộ server
│   └── paper-world-defaults.yml   ← Mặc định cho tất cả thế giới
└── world/
    └── paper-world.yml            ← Ghi đè theo từng thế giới riêng
```

---

## 🌐 paper-global.yml

### `anticheat.obfuscation.items` — Ẩn thông tin item

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enable-item-obfuscation` | `false` | Ẩn dữ liệu item (enchant, lore...) khỏi client của người chơi khác. Ngăn hack client scan item của người xung quanh. **Có thể làm hỏng resource pack.** |
| `sanitize-count` | `true` | Ẩn số lượng item trong tay người chơi khác. |
| `also-obfuscate` | `[]` | Thêm thành phần dữ liệu cần ẩn. Không nên chỉnh nếu không hiểu rõ. |
| `dont-obfuscate` | `[minecraft:lodestone_tracker]` | Thành phần **không** bị ẩn. Lodestone tracker được miễn vì ẩn nó sẽ làm la bàn bị rung loạn. |

> **Elytra:** Mặc định không ẩn `minecraft:damage` vì elytra 1 độ bền có texture nứt vỡ — cần giữ nguyên để hiển thị đúng.

---

### `block-updates` — Cập nhật block

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `disable-chorus-plant-updates` | `false` | Tắt cập nhật trạng thái cây chorus. Chỉ dùng cho map maker muốn tạo cấu hình cây bất thường. |
| `disable-mushroom-block-updates` | `false` | Tắt cập nhật block nấm. Chỉ dùng cho map maker. |
| `disable-noteblock-updates` | `false` | Tắt cập nhật note block. Chỉ dùng cho map maker. |
| `disable-tripwire-updates` | `false` | Tắt cập nhật dây bẫy. Chỉ dùng cho map maker. |

> ⚠️ Các tuỳ chọn này chỉ hữu ích cho server adventure/map. Server survival thì để `false` hết.

---

### `chunk-loading-advanced` — Tải chunk nâng cao

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `auto-config-send-distance` | `true` | Tự điều chỉnh khoảng gửi chunk theo view distance của từng client. Tiết kiệm băng thông. |
| `player-max-concurrent-chunk-generates` | `0` | Số chunk tạo mới tối đa cùng lúc mỗi người chơi. `0` = tự động, `-1` = không giới hạn. |
| `player-max-concurrent-chunk-loads` | `0` | Số chunk tải từ đĩa tối đa cùng lúc mỗi người chơi. `0` = tự động, `-1` = không giới hạn. |

---

### `chunk-loading-basic` — Tải chunk cơ bản

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `player-max-chunk-generate-rate` | `-1` | Tốc độ tạo chunk mới tối đa mỗi người (chunk/giây). `-1` = không giới hạn. Đặt `8–15` nếu server yếu hoặc nhiều người bay elytra. |
| `player-max-chunk-load-rate` | `100` | Tốc độ tải chunk từ đĩa tối đa mỗi người (chunk/giây). Giảm xuống `50` nếu có 20+ người để tránh quá tải ổ đĩa. |
| `player-max-chunk-send-rate` | `75` | Tốc độ gửi chunk xuống client tối đa mỗi người (chunk/giây). Giảm xuống `40–50` nếu băng thông yếu hoặc nhiều người join cùng lúc. |

---

### `chunk-system` — Hệ thống luồng chunk

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `io-threads` | `-1` | Số luồng đọc/ghi file chunk xuống ổ cứng. `-1` = 1 luồng. Tăng lên `2–3` nếu dùng HDD chậm. |
| `worker-threads` | `-1` | Số luồng tạo địa hình chunk mới. `-1` = tự tính = ½ số physical core (tối thiểu 1). Tăng nếu nhiều người explore cùng lúc. |

> **Lưu ý:** Hai luồng này chạy **độc lập** với main thread — server không bị block khi gen/load chunk.

---

### `collisions` — Va chạm

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enable-player-collisions` | `true` | Có cho phép người chơi va chạm vật lý với nhau không. Có thể xung đột với plugin scoreboard. |
| `send-full-pos-for-hard-colliding-entities` | `true` | Gửi vị trí chính xác cho thuyền/minecart để giảm lệch client-server. Tốn thêm băng thông nhưng giúp chuyển động mượt hơn. |

---

### `misc` — Linh tinh

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `chat-executor-core-size` | `-1` | Số luồng tối thiểu xử lý chat async. `-1` = 0 (dùng luồng hiện tại). |
| `chat-executor-max-size` | `-1` | Số luồng tối đa cho chat. `-1` = không giới hạn. |
| `compression-level` | `default` | Mức nén gói tin mạng. `default` = không nén. Đặt `1–6` để tiết kiệm băng thông (tốn thêm CPU). |
| `max-joins-per-tick` | `5` | Tối đa bao nhiêu người được vào server trong 1 tick. Người dư bị xếp hàng chờ, không bị kick. Tránh spike khi nhiều người vào cùng lúc. |
| `region-file-cache-size` | `256` | Số file region giữ mở trong RAM. Tăng lên `512` nếu thế giới rộng và có nhiều RAM. |
| `send-full-pos-for-item-entities` | `false` | Gửi vị trí chính xác cho item rơi xuống đất. Bật nếu item hay bị lệch vị trí. |
| `xp-orb-groups-per-area` | `default` | Số nhóm orb XP cùng giá trị tồn tại trong 1 khu vực trước khi gộp. Mặc định là 40. |

---

### `packet-limiter` — Giới hạn gói tin

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `max-packet-rate` | `500` | Số gói tin tối đa mỗi người trong khoảng thời gian `interval`. Bảo vệ server khỏi packet flood. |
| `interval` | `7.0` | Khoảng thời gian đếm gói tin (giây). |
| `action` | `KICK` | Hành động khi vượt giới hạn. `DROP` = bỏ qua gói tin thừa, `KICK` = kick người chơi. |

> Packet `minecraft:place_recipe` có thể cấu hình riêng — dùng để bắt người dùng spam recipe book.

---

### `player-auto-save` — Tự động lưu người chơi

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `rate` | `-1` | Tần suất lưu dữ liệu người chơi (tick). `-1` = dùng theo `bukkit.yml`. |
| `max-per-tick` | `-1` | Tối đa bao nhiêu người được lưu mỗi tick. `-1` = tự động (10 hoặc 20). Giảm xuống `5` trên server lớn để trải đều tải. |

---

### `spam-limiter` — Giới hạn spam

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `incoming-packet-threshold` | `300` | Số gói tin nhận vào trước khi bị coi là spam và bỏ qua. |
| `tab-spam-limit` | `500` | Số lần nhấn Tab trong chat trước khi bị kick vì spam. |
| `recipe-spam-limit` | `20` | Số lần click recipe trước khi bị kick vì spam. |

---

### `watchdog` — Canh gác server

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `early-warning-delay` | `10000` | Số millisecond server bị treo trước khi watchdog bắt đầu in thread dump (10 giây). |
| `early-warning-every` | `5000` | Khoảng cách giữa các lần in thread dump khi server đang treo (5 giây). |

> Watchdog theo dõi main thread. Nếu server không phản hồi quá lâu, nó in log để debug rồi force-crash để tránh server bị treo im lặng.

---

### `console` — Console server

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enable-brigadier-completions` | `true` | Bật tab-complete lệnh nâng cao trong console. |
| `enable-brigadier-highlighting` | `true` | Tô màu cú pháp lệnh trong console. |
| `has-all-permissions` | `false` | Nếu `true`, console bỏ qua mọi kiểm tra quyền. Để `false` trừ khi có lý do đặc biệt. |

---

### `item-validation` — Kiểm tra giới hạn item

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `book.author` | `8192` | Độ dài tối đa tên tác giả sách (ký tự). |
| `book.title` | `8192` | Độ dài tối đa tiêu đề sách (ký tự). |
| `book.page` | `16384` | Độ dài tối đa mỗi trang sách (ký tự). |
| `book-size.page-max` | `2560` | Số byte tối đa mỗi trang đóng góp vào tổng kích thước sách. `disabled` = bỏ giới hạn. |
| `book-size.total-multiplier` | `0.98` | Mỗi trang được phép dùng 0.98 lần byte của trang trước. Ngăn sách phình to theo cấp số nhân. |
| `display-name` | `8192` | Độ dài tối đa tên hiển thị của item (ký tự). |
| `lore-line` | `8192` | Độ dài tối đa mỗi dòng lore (ký tự). |
| `resolve-selectors-in-books` | `false` | Nếu `true`, các selector như `@a`, `@p` sẽ được xử lý trong sách. **Đừng bật** — người chơi creative có thể dùng để crash server. |

---

### `messages` — Tin nhắn hệ thống

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `kick.authentication-servers-down` | `<lang:...>` | Tin nhắn hiện khi server xác thực Mojang không kết nối được. Hỗ trợ MiniMessage. |
| `kick.connection-throttle` | `Connection throttled!...` | Tin nhắn khi người chơi reconnect quá nhanh. |
| `kick.flying-player` | `<lang:...>` | Tin nhắn khi kick người chơi vì bay. |
| `kick.flying-vehicle` | `<lang:...>` | Tin nhắn khi kick người chơi vì cưỡi phương tiện bay. |
| `no-permission` | `<red>I'm sorry...` | Tin nhắn mặc định khi không có quyền. Plugin có thể ghi đè cho từng lệnh. Hỗ trợ MiniMessage. |
| `use-display-name-in-quit-message` | `false` | Dùng tên hiển thị (do plugin đặt) thay vì tên thật trong thông báo thoát. |

---

### `proxies` — Cấu hình proxy

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `bungee-cord.online-mode` | `true` | Phải khớp với `online-mode` của BungeeCord proxy. Xử lý UUID khi đứng sau proxy. |
| `proxy-protocol` | `false` | Chỉ bật nếu dùng HAProxy hoặc tương tự. Không liên quan đến BungeeCord/Velocity. |
| `velocity.enabled` | `false` | Bật Velocity Modern Forwarding. Bắt buộc nếu dùng Velocity làm proxy. |
| `velocity.online-mode` | `true` | Phải khớp với `online-mode` của Velocity proxy. |
| `velocity.secret` | `""` | Phải trùng với secret trong file `forwarding.secret` của Velocity. |

> ⚠️ Cấu hình sai proxy có thể cho phép người chơi giả mạo UUID (bỏ qua xác thực). Luôn đặt `online-mode` khớp với proxy của bạn.

---

### `scoreboards` — Bảng điểm

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `save-empty-scoreboard-teams` | `true` | Tự xoá team scoreboard rỗng do plugin để lại. Giữ `true` để login nhanh hơn. |
| `track-plugin-scoreboards` | `false` | Theo dõi scoreboard chỉ có dummy objective. Bật lên với plugin dùng nhiều scoreboard sẽ giảm hiệu năng. |

---

### `spark` — Profiler

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enabled` | `true` | Bật Spark profiler tích hợp sẵn. Dùng để debug lag. Giữ `true`. |
| `enable-immediately` | `false` | Bật Spark ngay từ lúc server khởi động (trước khi Done). Bật khi cần debug lag lúc startup. |

---

### `time` — Thời gian

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `affects-all-worlds` | `false` | Nếu `true`, lệnh `/time` ảnh hưởng tất cả thế giới cùng dimension type. Nếu `false`, chỉ ảnh hưởng thế giới hiện tại của người dùng lệnh. |

---

## 🌍 paper-world.yml

### `anticheat.anti-xray` — Chống X-Ray

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enabled` | `false` | Bật hệ thống chống X-Ray. |
| `engine-mode` | `1` | `1` = Thay quặng bằng đá giả (nhẹ CPU). `2` = Tạo quặng giả ngẫu nhiên (nặng hơn, bảo mật hơn). `3` = Ngẫu nhiên theo từng lớp chunk. |
| `max-block-height` | `64` | Độ cao Y tối đa anti-xray hoạt động. Đặt `320` để phủ toàn bộ thế giới. |
| `lava-obscures` | `false` | Ẩn block tiếp xúc dung nham. Hoạt động kém với texture quặng không chuẩn. |

> **Lưu ý hiệu năng:** Engine mode `2` tốn CPU đáng kể. Dùng mode `1` nếu không cần bảo mật cao.

---

### `chunks` — Chunk

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `auto-save-interval` | `default` | Chu kỳ lưu thế giới (tick). `default` = dùng theo `bukkit.yml`. |
| `delay-chunk-unloads-by` | `10s` | Thời gian chờ trước khi unload chunk sau khi người chơi rời đi. Tránh load/unload liên tục khi đi qua lại ranh giới chunk. |
| `max-auto-save-chunks-per-tick` | `24` | Số chunk tối đa được lưu mỗi tick khi autosave. Giảm xuống `8–12` để tránh TPS bị giật lúc save. |
| `flush-regions-on-save` | `false` | Ép ghi hết dữ liệu chunk xuống đĩa mỗi lần save. Rất an toàn nhưng chậm. Chỉ bật nếu bị mất dữ liệu chunk. |
| `prevent-moving-into-unloaded-chunks` | `false` | Ngăn người chơi di chuyển vào chunk chưa được tải. Tránh một số crash hiếm gặp. |
| `entity-per-chunk-save-limit` | `-1` | Giới hạn số entity cùng loại được lưu mỗi chunk. Hữu ích khi farm mob tạo ra quá nhiều entity làm hỏng chunk. |

---

### `collisions` — Va chạm entity

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `max-entity-collisions` | `8` | Số va chạm tối đa được xử lý mỗi entity mỗi tick. Giảm xuống `2` trên server nhiều mob để tăng hiệu năng. |
| `only-players-collide` | `false` | Chỉ tính va chạm khi có người chơi liên quan. Giảm CPU trên server nhiều mob. |
| `allow-player-cramming-damage` | `false` | Người chơi bị damage khi bị nhét quá nhiều entity (như vanilla). |

---

### `entities` — Entity

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `armor-stands.tick` | `true` | Có cho armor stand tick mỗi game tick không. Tắt đi nếu có nhiều armor stand trang trí để tăng hiệu năng đáng kể. |
| `zombies-target-turtle-eggs` | `true` | Zombie liên tục quét block xung quanh tìm trứng rùa. Đặt `false` để giảm tải CPU nhẹ. |
| `baby-zombie-movement-modifier` | `0.5` | Hệ số tốc độ baby zombie. `0.5` = nhanh hơn 50% so với zombie thường. |
| `phantoms-do-not-spawn-on-creative-players` | `true` | Phantom không tấn công người chơi ở chế độ creative. |
| `nerf-pigmen-from-nether-portals` | `false` | Piglin/Pigmen sinh từ portal không có AI. Hữu ích nếu portal tạo quá nhiều piglin. |
| `experience-merge-max-value` | `-1` | Giới hạn giá trị tối đa của orb XP khi gộp. `-1` = không giới hạn (gộp thành 1 orb). Đặt `50` để giữ orb phân tán hơn. |

---

### `environment` — Môi trường

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `optimize-explosions` | `false` | Cache entity lookup khi xử lý vụ nổ thay vì tính lại mỗi lần. **Nên bật** — tăng tốc TNT/Creeper đáng kể. |
| `max-block-ticks` | `65536` | Số block update tối đa mỗi tick. Ngăn server bị treo khi có chuỗi update block vô hạn. |
| `max-fluid-ticks` | `65536` | Tương tự, cho nước/dung nham. |
| `disable-ice-and-snow` | `false` | Tắt tạo băng và tuyết. Cũng ngăn cauldron đầy nước khi mưa. |
| `nether-ceiling-void-damage-height` | `disabled` | Người chơi ở Nether trên độ cao này sẽ bị damage void. Dùng để chặn người xây trên trần Nether. |

---

### `hopper` — Phễu (Hopper)

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `cooldown-when-full` | `true` | Khi phễu đầy, thêm cooldown ngắn thay vì liên tục thử hút item. Giữ `true`. |
| `disable-move-event` | `false` | Tắt hoàn toàn sự kiện `InventoryMoveItemEvent` của hopper. **Tăng hiệu năng hopper rất mạnh** nhưng làm hỏng plugin bảo vệ chest. Chỉ bật nếu không dùng plugin loại đó. |
| `ignore-occluding-blocks` | `false` | Phễu bỏ qua container bên trong block đặc (vd: hopper minecart trong cát). Tăng hiệu năng nhẹ. |

---

### `misc` — Linh tinh (World)

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `redstone-implementation` | `VANILLA` | Engine xử lý redstone. `ALTERNATE_CURRENT` hoặc `EIGENCRAFT` giảm lag redstone farm rất nhiều. **Khuyến nghị cho server có farm.** Lưu ý: hành vi có thể khác vanilla đôi chút. |
| `update-pathfinding-on-block-update` | `true` | Tính lại đường đi của mob mỗi khi có block thay đổi. Đặt `false` trên server nhiều mob hoặc có redstone farm — gần như không ảnh hưởng gameplay. |
| `disable-end-credits` | `false` | Bỏ qua màn hình credits khi thoát khỏi The End. |

---

### `spawning` — Sinh vật

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `per-player-mob-spawns` | `true` | Tính giới hạn mob theo từng người thay vì toàn server. Phân bổ mob công bằng hơn. Giữ `true`. |
| `alt-item-despawn-rate` | `false` | Cho phép đặt thời gian biến mất khác nhau cho từng loại item. Bật để item rác (đá, đất) biến mất nhanh hơn. |
| `count-all-mobs-for-spawning` | `false` | Nếu `true`, mob từ spawner cũng tính vào giới hạn mob toàn server. Giữ `false` trừ khi spawner bị lạm dụng. |

**Ví dụ config item biến mất nhanh:**
```yaml
spawning:
  alt-item-despawn-rate:
    enabled: true
    items:
      cobblestone: 300    # 15 giây
      dirt: 300
      gravel: 300
      netherrack: 300
      sand: 300
```

---

### `spawning.despawn-ranges` — Khoảng cách despawn mob

Kiểm soát khoảng cách từ người chơi mà mob bị xoá. Có 2 ngưỡng:

| Ngưỡng | Ý nghĩa |
|--------|---------|
| `soft` | Ngoài khoảng này, mob có **xác suất** biến mất mỗi tick |
| `hard` | Ngoài khoảng này, mob **lập tức** bị xoá |

```yaml
despawn-ranges:
  monster:
    soft:
      horizontal: 32
      vertical: 16
    hard:
      horizontal: 128
      vertical: 64
```

> Giảm `hard` range trên server nhiều mob giúp giảm số entity và tăng TPS.

---

### `spawning.despawn-time` — Thời gian tồn tại entity

Buộc entity biến mất sau một khoảng thời gian cố định dù có người chơi gần hay không.

```yaml
despawn-time:
  villager: 1200   # Dân làng biến mất sau 60 giây nếu không có ai gần
```

> Hữu ích để dọn dẹp mob thừa từ farm hoặc sự kiện.

---

### `tick-rates` — Tần suất tick

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `mob-spawner` | `1` | Tần suất spawner thử spawn mob (tick). `1` = mỗi tick. Tăng lên `2–4` để giảm tải CPU từ spawner. |
| `grass-spread` | `1` | Độ trễ giữa các lần cỏ lan sang đất (tick). Tăng lên `4` để tăng hiệu năng nhẹ, không ảnh hưởng gameplay. |
| `container-update` | `1` | Tần suất đồng bộ inventory xuống client. **Không tăng** — sẽ gây ghost item và visual desync. |
| `dry-farmland` | `1` | Tần suất đất khô kiểm tra độ ẩm. `-1` = tắt. |
| `wet-farmland` | `1` | Tần suất đất ướt kiểm tra độ ẩm. `-1` = tắt. |

---

### `tracking-range-y` — Phạm vi theo dõi entity theo chiều dọc

Kiểm soát khoảng cách **dọc** để server gửi entity xuống client. Mặc định tắt — tất cả dùng phạm vi ngang.

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `enabled` | `false` | Bật phạm vi theo dõi dọc riêng. Hữu ích cho công trình cao hoặc farm dưới lòng đất. |
| `monster` | `default` | Phạm vi dọc theo dõi mob hostile. |
| `animal` | `default` | Phạm vi dọc theo dõi động vật. |
| `player` | `default` | Phạm vi dọc theo dõi người chơi. |
| `display` | `default` | Phạm vi dọc theo dõi display entity. |
| `misc` | `default` | Phạm vi dọc theo dõi entity khác. |

---

### `fixes` — Vá lỗi & Exploit

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `disable-unloaded-chunk-enderpearl-exploit` | `false` | Ngăn ender pearl lưu người ném khi bay vào chunk chưa tải. Bật để đóng exploit này. |
| `falling-block-height-nerf` | `disabled` | Xoá block rơi trên độ cao Y này. Hữu ích để ngăn pháo cát/sỏi. |
| `fix-items-merging-through-walls` | `false` | Ngăn item gộp qua tường. Tốn thêm CPU — chỉ cần khi `merge-radius` trong `spigot.yml` quá lớn. |
| `prevent-tnt-from-moving-in-water` | `false` | Ngăn TNT đã kích nổ trôi theo dòng nước. |
| `tnt-entity-height-nerf` | `disabled` | Xoá TNT đã kích nổ trên độ cao Y này. |
| `split-overstacked-loot` | `true` | Tách item stack vượt giới hạn từ loot table. Giữ `true` để tránh gói tin quá lớn làm hỏng chunk. |

---

### `lootables` — Rương loot tự hồi phục

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `auto-replenish` | `false` | Tự động đổ lại loot vào rương/thùng theo thời gian. Hữu ích cho server survival lâu dài không explore chunk mới. |
| `max-refills` | `-1` | Số lần tối đa rương được đổ lại. `-1` = vô hạn. |
| `refresh-min` | `12h` | Thời gian tối thiểu trước khi rương có thể được đổ lại. |
| `refresh-max` | `2d` | Thời gian tối đa trước khi rương bị đổ lại. |
| `reset-seed-on-fill` | `true` | Ngẫu nhiên hoá loot mỗi lần đổ lại thay vì lặp lại items cũ. |
| `restrict-player-reloot` | `true` | Ngăn cùng một người loot lại cùng một rương sau khi đổ lại. |
| `restrict-player-reloot-time` | `disabled` | Thời gian chờ riêng mỗi người giữa 2 lần loot. |

---

### `maps` — Bản đồ

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `item-frame-cursor-limit` | `128` | Số marker tối đa trên 1 bản đồ. Quá nhiều marker làm lag client đang render bản đồ. |
| `item-frame-cursor-update-interval` | `10` | Tần suất cập nhật cursor bản đồ trong item frame (tick). Đặt `0` hoặc âm để tắt cập nhật. |

---

### `max-growth-height` — Giới hạn chiều cao cây

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `bamboo.max` | `16` | Chiều cao tối đa tre mọc tự nhiên. |
| `bamboo.min` | `11` | Chiều cao tối thiểu tre mọc tự nhiên. |
| `cactus` | `3` | Chiều cao tối đa xương rồng mọc tự nhiên. |
| `reeds` | `3` | Chiều cao tối đa mía mọc tự nhiên. |

---

### `fishing-time-range` — Thời gian câu cá

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `minimum` | `100` | Số tick tối thiểu trước khi cá cắn câu. Giảm = câu nhanh hơn. |
| `maximum` | `600` | Số tick tối đa trước khi cá cắn câu. |

---

### `feature-seeds` — Seed tính năng thế giới

Cho phép gắn seed cố định cho các tính năng sinh thế giới (ore, structure...) để tái tạo hoặc tối ưu farm.

```yaml
feature-seeds:
  generate-random-seeds-for-all: false
  minecraft:ore_diamond: 12345
```

> `generate-random-seeds-for-all: true` tự điền seed ngẫu nhiên cho tất cả tính năng — hữu ích để xem có những option nào.

---

### `unsupported-settings` — Cài đặt không được hỗ trợ chính thức

> ⚠️ PaperMC **không bảo hành** các tuỳ chọn này. Dùng trên rủi ro của bạn. Có thể bị xoá bất cứ lúc nào.

| Tuỳ chọn | Mặc định | Giải thích |
|----------|----------|------------|
| `disable-world-ticking-when-empty` | `false` | Dừng tick thế giới hoàn toàn khi không có người chơi nào. Tốt cho server multi-world với thế giới ít người ghé thăm. |
| `fix-invulnerable-end-crystal-exploit` | `true` | Ngăn tạo end crystal bất tử. Giữ `true`. |

---

## 📌 Gợi Ý Config Nhanh Theo Loại Server

### Tất cả server
```yaml
# paper-world.yml
environment:
  optimize-explosions: true        # Tăng tốc TNT/Creeper
misc:
  update-pathfinding-on-block-update: false  # Giảm tải khi có redstone/farm
entities:
  armor-stands:
    tick: false                    # Tắt tick armor stand trang trí
  behavior:
    zombies-target-turtle-eggs: false  # Giảm tải nhỏ
collisions:
  max-entity-collisions: 2         # Giảm từ 8 xuống
chunks:
  max-auto-save-chunks-per-tick: 10  # Tránh TPS giật lúc autosave
```

### Server farm / redstone
```yaml
misc:
  redstone-implementation: ALTERNATE_CURRENT  # Giảm lag redstone mạnh
hopper:
  disable-move-event: true   # Chỉ bật nếu KHÔNG dùng plugin bảo vệ chest
tick-rates:
  mob-spawner: 2             # Spawner tick ít hơn
spawning:
  alt-item-despawn-rate:
    enabled: true
    items:
      cobblestone: 300
      dirt: 300
      gravel: 300
```

### Server 20+ người chơi
```yaml
# paper-global.yml
chunk-loading-basic:
  player-max-chunk-load-rate: 50    # Giảm tải IO
  player-max-chunk-send-rate: 45    # Giảm băng thông
  player-max-chunk-generate-rate: 12

# paper-world.yml
chunks:
  max-auto-save-chunks-per-tick: 10
```

---

## 🔗 Tài Liệu Tham Khảo

- [PaperMC Docs chính thức](https://docs.papermc.io/paper/reference/configuration/)
- [Global Config Reference](https://docs.papermc.io/paper/reference/global-configuration/)
- [World Config Reference](https://docs.papermc.io/paper/reference/world-configuration/)
- [Alternate Current (Redstone)](https://github.com/SpaceWalkerRS/alternate-current)
- [Spark Profiler](https://spark.lucko.me/) — tìm nguyên nhân lag

---

*Dựa trên tài liệu chính thức PaperMC. Cập nhật lần cuối: Tháng 6/2026.*
