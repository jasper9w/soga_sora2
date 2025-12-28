# Soga Sora2 - 视频批量生成工具

一个基于 Sora API 的视频批量生成工具，支持从 Excel 配置文件读取任务并行生成视频。

服务端基于：
https://github.com/genz27/Modified_Sora2ap

## 功能特性

- 📝 **Excel 配置** - 使用 Excel 表格管理视频生成任务
- 🎭 **角色映射** - 支持在提示词中使用 `@角色名` 引用预定义角色
- 🚀 **并发生成** - 支持多任务并行处理，提高生成效率
- 🖼️ **图生视频** - 支持使用参考图片生成视频
- 📊 **两种模式** - 顺序模式和均衡模式，灵活控制任务执行顺序
- ✅ **自动去重** - 已生成的视频自动跳过，避免重复生成

## 安装

```bash
# 使用 uv 安装（推荐）
uv pip install -e .

# 或使用 pip 安装
pip install -e .
```

## 使用方法

### 基本用法

```bash
soga_sora2 任务配置.xlsx
```

### 高级用法

```bash
soga_sora2 任务配置.xlsx \
  --api-url http://localhost:9200/v1/videos \
  --api-key your_api_key \
  --output-dir 输出目录 \
  --workers 10 \
  --interval 1.0 \
  --mode balanced
```

### 参数说明

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `excel_path` | Excel 配置文件路径（必需） | - |
| `--api-url` | Sora API 地址 | `http://localhost:9200/v1/videos` |
| `--api-key` | API 密钥 | `han1234` |
| `--output-dir` | 视频输出目录 | `视频生成结果` |
| `--workers` | 并发线程数 | `10` |
| `--interval` | 新任务提交间隔（秒） | `1.0` |
| `--mode` | 执行模式（`balanced`/`sequential`） | `balanced` |

### 执行模式

- **balanced（均衡模式）**：不同任务交替执行，避免单个任务占用过多资源
- **sequential（顺序模式）**：按任务编号顺序依次执行

## Excel 配置格式

Excel 文件需要包含两个工作表：

### 1. 任务表（sheet名: `任务`）

| 编号 | 提示词 | 生成数量 | 时长 | 尺寸 | 图片 |
|------|--------|----------|------|------|------|
| 001 | A @character walking in the park | 3 | 5 | 1920x1080 | path/to/image.jpg |
| 002 | @scene with beautiful lighting | 2 | 10 | 1280x720 | |

字段说明：
- **编号**：任务唯一标识
- **提示词**：视频生成的描述文本，支持使用 `@角色名` 引用角色
- **生成数量**：该任务生成的视频数量
- **时长**：视频时长（秒）
- **尺寸**：视频分辨率
- **图片**：参考图片路径（可选，用于图生视频）

### 2. 角色表（sheet名: `角色`）

| 角色名 | 角色值 |
|--------|--------|
| character | a cute cartoon cat |
| scene | sunset beach scene |

字段说明：
- **角色名**：在提示词中使用的角色标识（只能包含英文字母）
- **角色值**：角色的实际描述文本

## 输出结果

生成的视频文件命名格式：`{编号}_{序号}.mp4`

例如：
- `001_001.mp4`
- `001_002.mp4`
- `002_001.mp4`

## 技术栈

- Python 3.12+
- pandas - Excel 文件读取
- requests - API 调用和视频下载

## 开发

```bash
# 克隆项目
git clone <repository-url>
cd soga-sora2

# 安装开发依赖
uv pip install -e .

# 运行
soga_sora2 --help
```

## License

MIT
