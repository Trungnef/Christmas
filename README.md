# 🎄 Magic Christmas - Hệ Thống Tương Tác Camera 3D

Ứng dụng web tương tác cao sử dụng **Three.js** và **MediaPipe Hands** để tạo trải nghiệm Giáng sinh thần kỳ với điều khiển bằng cử chỉ tay.

---

## 📋 Tổng Quan Hệ Thống

```
Magic Christmas (3D Interactive Web App)
├── Background: Aurora (Cực Quang) + Gradient + Stars
├── Main Scene: Christmas Tree (Cây Thông Noel)
├── Interactive States:
│   ├── TREE: Hiển thị cây thông
│   ├── HEART: Hiển thị chữ "I Love You" (gesture tim)
│   ├── EXPLODE: Phân tán hạt, xoay ảnh
│   └── PHOTO_ZOOM: Phóng to ảnh được chọn
└── Hand Gesture Control: Camera + MediaPipe Hands API
```

---

## 🎯 Các Tính Năng Chính

### 1. **Hiệu Ứng Nền (Aurora Background)**
- **Shader GLSL**: Perlin noise tạo hiệu ứng cực quang động
- **Gradient**: Đen (dưới) → Xanh tím (trên)
- **Màu sắc**: Cyan + Purple blend với aurora effect

### 2. **Cây Thông Noel (Christmas Tree)**
- **Hình dạng**: Hình xoắn ốc 3D với 4 cánh tay
- **Chiều cao**: 75 đơn vị, bán kính cơ sở: 25 đơn vị
- **Hiệu ứng lần lượt**: Từng phần cây hiển thị khi sao băng chạy qua (shooting stars)

### 3. **Hệ Thống Hạt (Particle Systems)**

| Loại | Số Lượng | Mục Đích |
|------|----------|---------|
| **Main Particles** | 2500 | Hạt chính xanh tím |
| **Sub Particles** | 5000 | Hạt phụ trắng nhỏ |
| **Dust Particles** | 1000 | Bụi lơ lửng |
| **Base Dust** | 4000 | Bụi ở chân cây |
| **Snow** | 1500 | Tuyết rơi |
| **Background Stars** | 2000 | Sao ở nền |
| **Shooting Stars** | 4 | Sao băng (startup animation) |
| **Galaxy** | 7000 | Thiên hà xoay (EXPLODE state) |

### 4. **Văn Bản 3D (Text Meshes)**

#### Merry Christmas (Trắng Sáng)
- **Vị trí**: Trên đỉnh cây
- **Hiệu ứng**:
  - Gradient stroke (Xanh → Vàng → Xanh)
  - 2 lớp glow (Trắng + Xanh lam)
  - Animation: Nổi, phóng to, xoay, nhấp nháy

#### I Love You (Đỏ Sinh Động)
- **Vị trí**: Dưới cây khi gesture tim
- **Hiệu ứng**:
  - Gradient đỏ (Đỏ → Hồng → Đỏ)
  - 2 lớp glow (Hồng + Đỏ sâu)
  - Animation: Đập tim, nhảy, xoay, nhấp nháy

### 5. **Thư Viện Ảnh (Photo Gallery)**
- **Hình ảnh**: image1.jpg → image5.jpg (5 ảnh)
- **Hiệu ứng quay**: Xoay vòng quanh tâm
- **Tương tác**: Vẫy tay để xoay, pinch để phóng to

---

## 🎮 Điều Khiển Cử Chỉ Tay

Sử dụng **MediaPipe Hands** để nhận diện 21 điểm tay (landmarks)

### Các Cử Chỉ:

| Cử Chỉ | Nhận Diện | Hiệu Ứng |
|--------|-----------|---------|
| **Mở Tay** (5 ngón) | Khoảng cách đầu ngón > 0.25 | **EXPLODE** - Phân tán hạt, xoay ảnh |
| **Nắm Tay** (Pinch) | Khoảng cách cái ngón - ngón trỏ < 0.05 | **PHOTO_ZOOM** - Phóng to ảnh |
| **Tim** (2 tay close) | Khoảng cách 2 tay < 0.1 | **HEART** - Hiển thị "I Love You" |
| **Vẫy Tay** (deltaX) | Di chuyển tay trái/phải | Xoay ảnh trong EXPLODE/ZOOM state |

---

## 🎵 Âm Thanh

- **File**: `Christmas.mp3` hoặc `audio.mp3`
- **Phát**: Khi nhấn nút "Start"
- **Loop**: Lặp lại liên tục
- **Âm lượng**: 100%

---

## 📁 Cấu Trúc File

```
Project/
├── index.html              # Main HTML file (1092 dòng)
├── Christmas.mp3 / audio.mp3  # Nhạc nền
├── image1.jpg - image5.jpg # Ảnh cho photo gallery
└── README.md              # Tài liệu này
```

### Thư Viện Extern (CDN):
- **Three.js r128** - 3D rendering
- **MediaPipe Hands** - Hand gesture detection
- **Google Fonts** - Great Vibes + Quicksand

---

## 🚀 Hướng Dẫn Chạy

### Yêu Cầu
- Trình duyệt hiện đại (Chrome, Firefox, Edge)
- Camera (webcam)
- Kết nối Internet (CDN libraries)

### Cách Chạy Local

**Option 1: Python (đơn giản nhất)**
```bash
cd /path/to/project
python -m http.server 8000
# Truy cập: http://localhost:8000
```

**Option 2: Node.js**
```bash
npx http-server . -p 8000
# Truy cập: http://localhost:8000
```

**Option 3: VS Code Live Server**
- Cài extension `Live Server`
- Mở `index.html` → Click phải → `Open with Live Server`

### Lưu Ý Quan Trọng
- ⚠️ **Camera chỉ hoạt động trên HTTPS hoặc localhost**
- Cho phép quyền truy cập camera khi trình duyệt hỏi
- Độ sáng phòng: Bình thường để nhận diện tốt

---

## 🔧 Cấu Hình Kỹ Thuật

### CONFIG Chính
```javascript
const CONFIG = {
    mainCount: 2500,        // Hạt chính
    subCount: 5000,         // Hạt phụ
    dustCount: 1000,        // Bụi
    baseDustCount: 4000,    // Bụi ở chân cây
    snowCount: 1500,        // Tuyết
    bgStarCount: 2000,      // Sao nền
    
    // Sao Băng
    shootingStarCount: 4,
    shootingStarSpeed: 0.004,
    trailLength: 30,
    
    // Cây Thông
    treeHeight: 75,
    treeBaseRadius: 25,
    spiralLoops: 4.2,
    treeYOffset: 15,
    
    // Thiên Hà (EXPLODE)
    galaxyCount: 7000,
    galaxyRadius: 220,
    galaxySpeed: 0.25,
    
    // Ảnh
    photoOrbitRadius: 25,
};
```

### Camera & Render
- **FOV**: 60°
- **Aspect**: window.innerWidth / window.innerHeight
- **Near**: 0.1, **Far**: 2000
- **Pixel Ratio**: Min(window.devicePixelRatio, 2)

---

## 🎨 Color Palette

| Phần | Màu Chính | RGB |
|-----|-----------|-----|
| Cây Thông | Xanh Tím | #8A2BE2 |
| Hạt Phụ | Trắng | #FFFFFF |
| Merry Christmas | Trắng + Xanh | #FFFFFF / #00BFFF |
| I Love You | Đỏ Hồng | #FF1744 / #FF69B4 |
| Nền | Đen Gradient | #000000 → #050f35 |

---

## 🎬 State Machine

```
TREE (Initial)
  ├─→ HEART (2 tay close, khoảng cách < 0.1)
  └─→ EXPLODE (tay mở, 5 ngón visible)
       ├─→ PHOTO_ZOOM (pinch: cái - trỏ < 0.05)
       │   └─→ EXPLODE (tay mở lại)
       └─→ TREE (nắm tay lại, 5 ngón < 0.25)
```

---

## 📊 Animation & Loop

### Main Loop (animate())
- **FPS**: 60 FPS (requestAnimationFrame)
- **Time**: `Date.now() * 0.001` (giây)

### Hiệu Ứng Chính
1. **Particle Update** - Dịch chuyển từ source → target (TREE/HEART/EXPLODE)
2. **Aurora Update** - Perlin noise animation (liên tục)
3. **Galaxy Update** - Xoay, rút vào từ từ
4. **Snow Update** - Rơi xuống + drift theo sin wave
5. **Text Animation** - Scale pulse, float, rotate

---

## 🛠️ Các Hàm Chính

### Initialization
- `init3D()` - Khởi tạo scene, camera, renderer
- `createAurora()` - Tạo nền cực quang
- `createParticleSystem()` - Tạo hệ thống hạt
- `createPhotos()` - Tạo ảnh 3D
- `createDecorations()` - Tạo text (Merry Christmas, I Love You)

### Updates
- `updateParticleGroup()` - Cập nhật vị trí hạt
- `updateShootingStarSystem()` - Sao băng startup
- `updateGalaxySystem()` - Thiên hà xoay
- `updateSnowSystem()` - Tuyết rơi
- `updateBaseDust()` - Bụi chân cây
- `animate()` - Main render loop

### Input
- `startSystem()` - Khởi động (click button)
- `hands.onResults()` - Xử lý hand detection
- State transitions dựa trên landmarks

---

## 🎯 Tối Ưu & Performance

- **Additive Blending** - Hiệu ứng glow nhẹ hơn
- **DepthWrite: false** - Bỏ qua depth buffer cho particle
- **Vertex Colors** - Tô màu trực tiếp trên buffer
- **Canvas Texture** - Tạo texture động (text, glow)
- **Fog**: FogExp2(0x00021A, 0.001) - Tạo độ sâu

---

## 🐛 Troubleshooting

| Vấn Đề | Nguyên Nhân | Giải Pháp |
|--------|-------------|----------|
| Camera không hoạt động | HTTPS/localhost không đúng | Dùng local server |
| Tay không nhận diện | Ánh sáng yếu | Tăng độ sáng phòng |
| Ảnh không tải | Missing image files | Kiểm tra image1-5.jpg |
| Âm thanh không nghe | File không tồn tại | Kiểm tra Christmas.mp3 |
| FPS thấp | Quá nhiều hạt | Giảm CONFIG.***Count |

---

## 📝 Ghi Chú

- Dự án sử dụng **Real-time Shader** cho nền aurora
- Hand detection dựa trên **MediaPipe ML model** (tải CDN)
- Hạt có **state-based animation** (TREE/HEART/EXPLODE)
- Text được render trên **canvas** rồi convert thành texture 3D

---

## 📄 License & Credits

- **Three.js** - mrdoob
- **MediaPipe** - Google
- **Canvas & WebGL** - Browser APIs

---

**Happy Holidays! 🎄✨**