# Vimでlaunch ファイルをxmlのフォーマットで読み込む

```bash
cd ~/.vim
mkdir ftdetect
vim launch.vim
```

```vim
".launchを開くときはファイルタイプをxmlにする
au BufNewFile,BufRead *.launch setf xml
```

- 出展：[vimが自動認識するファイルタイプを自分で設定する](https://syguer.hatenablog.com/entry/2013/12/25/002023)

# ros_control
## 指令値テスト
```bash
rostopic pub /fr01/steer_right_front_joint_position_controller/command std_msgs/Float64 -r 1 -- -1
```

# Compress Image
## Python
- http://wiki.ros.org/ja/rospy_tutorials/Tutorials/WritingImagePublisherSubscriber

## C++
- 情報がなさすぎるのでサンプルをメモしておく。たったコレだけのことなのに…。
```cpp
void hogeClass::CompressedImageSubscribe (const sensor_msgs::CompressedImagePtr & compress_img_msg)
{
    cv::Mat decoded_img;
    decoded_img = cv::imdecode(cv::Mat(compress_img_msg->data), 1);
}
```

# Learning ROS for Robotics Programming
## `catkin_make`した時、`opencv2/nonfree/features2d.hpp no such file or directory.`と怒られる。
```bash
sudo add-apt-repository --yes ppa:xqms/opencv-nonfree
sudo apt-get update
sudo apt-get install libopencv-nonfree-dev
```

# Matlab on Windows
## kubuntu のturtlebotと繋ぐ
- kubuntu
  - `gazebo TrutleBot World` を開く。コンソールに色々出てきてgazeboが立ち上がる。
  - `Publicized address: 192.168.183.128` がgazeboっぽい。
  - というか、デスクトップの左上にでかーく書いてあった…。
- Windowsマシン
  - とりあえずつなげる
    - ネットワーク接続からVmWareNetworkの仮想ネットワークを全て有効にする。
    - ipconfig -> `192.168.183.1` で認識されているはず。
    - ping 192.168.183.128 で接続を確認。
    - Matlabコンソールから`rosinit(192.168.183.128)` をする。
    - `rostopic list` を実行。つながっているはず。
    - 適当にsubscribeは出来るはず。多分publishできない。
  - Fire Wallの設定
      - Fire Wall を解放しないとpublishだけできないっぽい。
      - VMと接続中のネットワーク(デフォはパブリックだった)のTCPを解放する。UDPは切っても切らなくても挙動は変わらない。
      - 運が良ければpublish時にWindowsが勝手にダイアログを出してくれるので、そのまま許可する。

# QtCreaterでパスが通らず補完できない
- 下記の`bash`を`source`する．
  ```bash
  #!/bin/bash

  # include path を作成する
  include_path=$(pwd)/include
  
  # CPATHに追加
  export CPATH=${CPATH}:${include_path}
  ```

# Gazebo でデバッグ
- http://wiki.ros.org/pr2_simulator/Tutorials/RunningSimulatorWithGDB

# OpenRAVE
- Motomanのdae情報出力例
```bash
[morita@ mesh] (add-ikfast-plugin *%)$ openrave-robot.py sia5.dae --info links
could not import scipy.optimize.leastsq
name      index parents  
-------------------------
world     0              
base_link 1     world    
link_s    2     base_link
link_l    3     link_s   
link_e    4     link_l   
link_u    5     link_e   
link_r    6     link_u   
link_b    7     link_r   
link_t    8     link_b   
tool0     9     link_t   
-------------------------

```

- IKFastのコマンド
```bash
python `openrave-config --python-dir`/openravepy/_openravepy_/ikfast.py --robot=sia5.dae --iktype=transform6d --baselink=1 --eelink=9 --freeindex=6 --savefile=output_ikfast92.cpp
```

output_ikfast92.cpp

```cpp
// l.15
/// ikfast version 0x10000048 generated on 2016-04-19 20:59:47.751149
↓
/// ikfast version 61 generated on 2016-04-19 20:59:47.751149

// l.26
IKFAST_COMPILE_ASSERT(IKFAST_VERSION==0x10000048);
↓
IKFAST_COMPILE_ASSERT(IKFAST_VERSION==61);
```

```bash
rosrun moveit_ikfast create_ikfast_moveit_plugin.py motoman_sia5 arm motoman_sia5_moveit_plugins motoman_sia5_arm_ikfast_solver.cpp 
```

# rosdep
```bash
cd hogehoge_ws
rosdep install hoge_package
```

こんなエラーが出る時がある．
```
ERROR: the following packages/stacks could not have their rosdep keys resolved
to system dependencies:
hoge_package: Cannot locate rosdep definition for [hoge_required_package]
```

そういう時は以下のコマンド
```bash
cd hogehoge_ws
rosdep update
rosdep install hoge_package
```

# Gazebo
Gazebのworldモデルは以下のコマンドでDLできる．

```bash
$ wget -r -R "index\.html*" http://models.gazebosim.org/
```

`models.gazebosim.org/`というフォルダに入ってしまう．

だから，中身は手動で`~/.gazebo/models`にコピーする．

- [What namespace does gazebo expects?](http://answers.gazebosim.org/question/6870/what-namespace-does-gazebo-expects/)
- [models.gazebosim.org/](http://models.gazebosim.org/)

# Test関連
- [How can I integrate my ROS package with Travis continuous integration?](http://answers.ros.org/question/220305/how-can-i-integrate-my-ros-package-with-travis-continuous-integration/)
