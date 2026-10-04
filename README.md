examples\training\train_policy.py    训练代码
    training_steps = 5000（迭代至少5000步）


在pycharm终端运行：
cd /d "C:\Users\23817\Desktop\新建文件夹\lerobot-0.6.1"
call setup_env.bat
.venv312\Scripts\activate.bat

 找串口（USB 转 TTL 适配器插上后）：
lerobot-find-port

设置电机 ID 和波特率（只做一次，按提示逐个连电机）
lerobot-setup-motors --robot.type=so101_follower --robot.port=COMx


校准：
lerobot-calibrate --robot.type=so101_follower --robot.port=COMx --robot.id=my_follower


摄像头插上电脑后先跑：
lerobot-find-cameras


录制数据（关键命令）：
cd /d "C:\Users\23817\Desktop\新建文件夹\lerobot-0.6.1"
call setup_env.bat
.venv312\Scripts\activate.bat

lerobot-record --robot.type=so101_follower --robot.port=COM3 --robot.cameras="{camera: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 30}}" --robot.id=my_follower --teleop.type=so101_leader --teleop.port=COM4 --teleop.id=my_leader --dataset.name=my_task
把 `COM3` / `COM4` 换成 `lerobot-find-port` 查到的两个串口（follower 和 leader 各一个）。

运行后：屏幕出现摄像头画面 → 你握着 leader 做动作 → follower 跟着动 → 按 C开始录制、再按C 停止、Q退出保存。

注：摄像头装在哪，画面就决定模型学什么：模型是 "看画面 → 推断动作"。如果镜头拍的是操作台 / 目标物体（follower 干活的地方），模型能学会任务；如果只拍到你的手握着 leader，模型可能学不到任务逻辑。建议把 leader 上的摄像头朝外对准工作台。

第一次跑之前，先完成两个前置步骤
lerobot-setup-motors --robot.type=so101_follower --robot.port=COM3
lerobot-calibrate --robot.type=so101_follower --robot.port=COM3 --robot.id=my_follower
         leader 也要对应跑一遍（`--teleop.type=so101_leader --teleop.port=COM4`）
