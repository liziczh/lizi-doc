---
title: Java | Tetris
comments: true
date: 2018-05-19 23:01:34
id: java-tetris
tags: 
- Java小程序
categories: Java小程序
toc: true
reward: true
copyright: true
---

<!--# Tetris-->

基于 Java-Swing 实现俄罗斯方块
GitHub：[https://github.com/liziczh/Tetris](https://github.com/liziczh/Tetris)

<!--more-->

## Preview

**普通模式**：

![Tetris](http://p6uturdzt.bkt.clouddn.com/tetris-NormalMode.PNG)

**加速模式**：

![Accel](http://p6uturdzt.bkt.clouddn.com/tetris-AccelMode.PNG)

## 设计思路

### 方块的属性

- 方块表示：
  - 由四维数组 `SHAPE[][][][]` 表示7种方块及每种方块的4种翻转状态。
  - 由`SHAPE[type][state]`可以唯一标识一个方块。
- 方块属性：
  - `type`：方块类型
  - `state`：方块翻转状态
  - `(x, y)`：方块坐标
  - `nextType`：下一方块类型
  - `nextState`：下一方块翻转状态
- 背景块：由`map[ROW][COl]`表示已固定的背景块。

### 方块的行为

**方块翻转**：通过改变方块的state属性实现翻转

```java
public void turn() {
  	int temp = state;
  	state = (state + 1) % 4;
  	// 如果旋转后不合法，还原上一状态
  	if (!check(type, state, x, y)) {
  		state = temp;
  	}
}
```

**方块下落**：通过改变方块纵坐标实现下落

```java
public void down() {
    // 如果下一个下落状态合法，则下落；不合法，则固定，清行，新建块。
  	if (check(type, state, x, y + 1)) {
  		y++;
  	} else {
  		fix(type, state, x, y);
  		clearLines();
  		createShape();
  	}
  	this.repaint();
}
```

**方块左移**：通过改变方块横坐标实现左移

```java
public void left() {
  	if (check(type, state, x - 1, y)) {
  		x--;
  	}
}
```

**方块右移**：通过改变方块横坐标实现右移

```java
public void right() {
	if (check(type, state, x + 1, y)) {
  		x++;
  	}
}
```

### 核心方法实现

**方块碰撞检测**：

```java
private boolean check(int type, int state, int x, int y) {
	for (int i = 0; i < SHAPE[type][state].length; i++) {
		for (int j = 0; j < SHAPE[type][state][0].length; j++) {
			if (SHAPE[type][state][i][j] == 1) {
				// 在坐标系中小方块坐标(x+j,y+i);在背景矩阵中小方块位置map[y+i][x+j];
				if ((x + j >= COL) || (x + j < 0) || (y + i >= ROW) || (map[y + i][x + j] == 1)) {
					return false;
				}
			}
		}
	}
	return true;
}
```

**固定当前方块到背景中**：

```java
private void fix(int type, int state, int x, int y) {
	for (int i = 0; i < SHAPE[type][state].length; i++) {
		for (int j = 0; j < SHAPE[type][state][0].length; j++) {
			// 在坐标系中小方块坐标(x+j,y+i);在背景矩阵中小方块位置map[y+i][x+j];
			if ((y + i < ROW) && (x + j >= 0) && (x + j < COL) && (map[y + i][x + j] == 0)) {
				map[y + i][x + j] = SHAPE[type][state][i][j];
				mapColor[y + i][x + j] = color[type];
			}
		}
	}
}
```

**清行加分**：

```java
private void clearLines() {
	int lines = 0;
	boolean isFull = true;
	for (int i = 0; i < map.length; i++) {
		isFull = true;
		for (int j = 0; j < map[0].length; j++) {
			if (map[i][j] == 0) {
				isFull = false;
				break;
			}
		}
		if (isFull) {
			lines++;
			for (int m = i; m > 0; m--) {
				for (int n = 0; n < map[0].length; n++) {
					map[m][n] = map[m - 1][n];
				}
			}
		}
	}
	score += lines * lines * 10;
	if (isAccelMode) {
		up();
	}
}
```

### GUI

绘制图形：重写paint()方法，使用Graphics类随意绘制。

Graphics类绘制方法如下：

```java
setColor(Color color);  // 设置画笔颜色
drawRect(int x,int y,int width,int height);  // 绘制矩形边框
fillRect(int x,int y,int width,int height);  // 填充矩形
fill3DRect(int x,int y,int width,int height, boolean raised); // 填充3D矩形。
drawLine(x1,y1,x2,y2);  // 绘制线条
setFont(Font font); // 设置字体
drawString(String text, int x, int y);  // 绘制字符串
```

## **文件目录**

com.lizich.tetris下：

```yaml
.
|——TetrisMain  # 游戏窗口，由此运行
|——TetrisCtrl  # 绘制游戏面板&游戏逻辑
```

com.lizi.tetris.singleclass下：

```yaml
.
|——Tetris # 合并了TetrisMain与TetrisCtrl
```



## 源码

**TetrisMain**

```java
package com.liziczh.tetris;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;

public class TetrisMain extends JFrame {

	/**
	 * 版本号
	 */
	private static final long serialVersionUID = 1L;
	// 游戏面板
	private TetrisCtrl tCtrl = new TetrisCtrl();

	/**
	 * 游戏窗口初始化
	 */
	public TetrisMain() {
		// 设置标题
		this.setTitle("Lizi Tetris");
		// 设置大小
		this.setSize(tCtrl.getSize());
		// 调用方法居中
		this.setLocationRelativeTo(null);
		// 设置关闭操作：关闭窗口，程序结束运行；
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		// 设置窗体大小不改变
		this.setResizable(false);
		// 添加键盘监听事件
		this.addKeyListener(keyListener);

		// 菜单栏
		JMenuBar menu = new JMenuBar();
		this.setJMenuBar(menu);
		JMenu gameMenu = new JMenu("游戏");
		JMenuItem newGameItem = gameMenu.add("新游戏");
		newGameItem.addActionListener(newGameAction);
		JMenuItem pauseItem = gameMenu.add("暂停");
		pauseItem.addActionListener(pauseAction);
		JMenuItem continueItem = gameMenu.add("继续");
		continueItem.addActionListener(continueAction);
		JMenuItem exitItem = gameMenu.add("退出");
		exitItem.addActionListener(exitAction);
		JMenu modeMenu = new JMenu("模式");
		JMenuItem normalModeItem = modeMenu.add("普通模式");
		normalModeItem.addActionListener(normalModeAction);
		JMenuItem accelModeItem = modeMenu.add("加速模式");
		accelModeItem.addActionListener(accelModeAction);
		JMenu helpMenu = new JMenu("帮助");
		JMenuItem aboutItem = helpMenu.add("关于");
		aboutItem.addActionListener(aboutAction);
		menu.add(gameMenu);
		menu.add(modeMenu);
		menu.add(helpMenu);
		// 设置窗口可见
		this.setVisible(true);
		// 添加TetrisPanel
		this.add(tCtrl);

	}

	// 键盘事件监听
	KeyListener keyListener = new KeyAdapter() {

		@Override
		public void keyPressed(KeyEvent e) {
			switch (e.getKeyCode()) {
			case KeyEvent.VK_UP:
				// ↑：旋转
				tCtrl.turn();
				tCtrl.repaint();
				break;
			case KeyEvent.VK_LEFT:
				// ←：左移
				tCtrl.left();
				tCtrl.repaint();
				break;
			case KeyEvent.VK_RIGHT:
				// →：右移
				tCtrl.right();
				tCtrl.repaint();
				break;
			case KeyEvent.VK_DOWN:
				// ↓：下移
				tCtrl.down();
				tCtrl.repaint();
				break;
			}
		}
	};

	// 新游戏
	ActionListener newGameAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent arg0) {
			tCtrl.init();
		}
	};
	// 暂停
	ActionListener pauseAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent arg0) {
			tCtrl.setPause();
		}
	};
	// 继续
	ActionListener continueAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent e) {
			tCtrl.setContinue();
		}
	};

	// 退出
	ActionListener exitAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent e) {
			System.exit(0);
		}
	};

	// 普通模式
	ActionListener normalModeAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent e) {
			tCtrl.setNormal();
		}
	};

	// 加速模式
	ActionListener accelModeAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent e) {
			tCtrl.setAccel();
		}
	};

	// 关于
	ActionListener aboutAction = new ActionListener() {

		@Override
		public void actionPerformed(ActionEvent e) {
			JOptionPane.showMessageDialog(tCtrl, "Tetris v1.0 from liziczh", "关于", getDefaultCloseOperation());
		}
	};

	public static void main(String[] args) {
		new TetrisMain();
	}

}

```

**TetrisCtrl**：

```java
package com.liziczh.tetris;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.Timer;

public class TetrisCtrl extends JPanel {

	/**
	 * 版本号
	 */
	private static final long serialVersionUID = 1L;

	// 方块边长，单位像素(px)
	public final int LEN = 24;

	// Panel区域：20行10列
	public final int ROW = 20;
	public final int COL = 10;

	/**
	 * SHAPE[type][state]：方块形状； type方块类型， state方块旋转状态；
	 */
	private final int[][][][] SHAPE = new int[][][][] {
			// S:
			{ { { 0, 1, 1, 0 }, { 1, 1, 0, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 0, 0, 0 }, { 1, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 1, 0 }, { 1, 1, 0, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 0, 0, 0 }, { 1, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } } },
			// Z:
			{ { { 1, 1, 0, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 0, 0 }, { 1, 1, 0, 0 }, { 1, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 1, 0, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 0, 0 }, { 1, 1, 0, 0 }, { 1, 0, 0, 0 }, { 0, 0, 0, 0 } } },
			// L:
			{ { { 1, 0, 0, 0 }, { 1, 0, 0, 0 }, { 1, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 1, 1, 0 }, { 1, 0, 0, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 0, 1, 0 }, { 1, 1, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } } },
			// J:
			{ { { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 1, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 0, 0, 0 }, { 1, 1, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 1, 0, 0 }, { 1, 0, 0, 0 }, { 1, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 1, 1, 1, 0 }, { 0, 0, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } } },
			// I:
			{ { { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 1, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 1, 1, 1, 1 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 1, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 1, 1, 1, 1 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } } },
			// O:
			{ { { 0, 0, 0, 0 }, { 0, 1, 1, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 0, 1, 1, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 0, 1, 1, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 0, 1, 1, 0 }, { 0, 1, 1, 0 }, { 0, 0, 0, 0 } } },
			// T:
			{ { { 0, 1, 0, 0 }, { 1, 1, 1, 0 }, { 0, 0, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 0, 0 }, { 0, 1, 1, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 0, 0, 0 }, { 1, 1, 1, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } },
					{ { 0, 1, 0, 0 }, { 1, 1, 0, 0 }, { 0, 1, 0, 0 }, { 0, 0, 0, 0 } } },

	};

	// 当前方块的四个参数：type方块类型，state方块旋转状态，坐标(x,y)。
	private int type, state, x, y;
	// 下一个块的参数：nextType方块类型，nextState方块旋转状态
	private int nextType, nextState;

	// 背景：已固定块
	private int[][] map = new int[ROW][COL];
	// 已固定块的颜色
	private Color[][] mapColor = new Color[ROW][COL];

	// 得分
	private int score = 0;
	// 等级
	private int level = 0;
	// 下落延时
	private int delay = 1000;

	// 是否在暂停状态
	private boolean isPause = false;

	// 是否为加速模式
	private boolean isAccelMode = false;

	// 方块颜色：color[type]
	private Color[] color = new Color[] { Color.green, Color.red, Color.orange, Color.blue, Color.cyan, Color.yellow,
			Color.magenta, Color.gray };

	public TetrisCtrl() {
		// 初始化Panel大小
		this.setSize(LEN * 20, LEN * 25);
		this.init();
	}

	/**
	 * 游戏初始化
	 */
	public void init() {
		// 初始化背景map
		for (int i = 0; i < map.length; i++) {
			for (int j = 0; j < map[0].length; j++) {
				map[i][j] = 0;
			}
		}
		// 初始化分数
		score = 0;
		isPause = false;
		// 随机生成下一方块
		nextType = (int) (Math.random() * 7);
		nextState = (int) (Math.random() * 4);
		// 生成当前方块
		createShape();

		// 启动timer
		timer.start();
		// 绘图
		this.repaint();
	}

	/**
	 * 创建一个新方块
	 */
	public void createShape() {
		// 当前块
		type = nextType;
		state = nextState;
		x = 3;
		y = 0;
		// 下一块
		nextType = (int) (Math.random() * 7);
		nextState = (int) (Math.random() * 4);
		// 如果新块不合法，则表示游戏已结束，则重新开始
		if (!check(type, state, x, y)) {
			JOptionPane.showMessageDialog(this, "GAME OVER!");
			init();
		}
	}

	/**
	 * 判断方块是否合法：true合法，false不合法
	 */
	private boolean check(int type, int state, int x, int y) {
		for (int i = 0; i < SHAPE[type][state].length; i++) {
			for (int j = 0; j < SHAPE[type][state][0].length; j++) {
				if (SHAPE[type][state][i][j] == 1) {
					// 在坐标系中小方块坐标(x+j,y+i);在背景矩阵中小方块位置map[y+i][x+j];
					if ((x + j >= COL) || (x + j < 0) || (y + i >= ROW) || (map[y + i][x + j] == 1)) {
						return false;
					}
				}
			}
		}
		return true;
	}

	/**
	 * 固定shape到map
	 */
	private void fix(int type, int state, int x, int y) {
		for (int i = 0; i < SHAPE[type][state].length; i++) {
			for (int j = 0; j < SHAPE[type][state][0].length; j++) {
				// 在坐标系中小方块坐标(x+j,y+i);在背景矩阵中小方块位置map[y+i][x+j];
				if ((y + i < ROW) && (x + j >= 0) && (x + j < COL) && (map[y + i][x + j] == 0)) {
					map[y + i][x + j] = SHAPE[type][state][i][j];
					mapColor[y + i][x + j] = color[type];
				}
			}
		}
	}

	/**
	 * 消行加分
	 */
	private void clearLines() {
		int lines = 0;
		boolean isFull = true;
		for (int i = 0; i < map.length; i++) {
			isFull = true;
			for (int j = 0; j < map[0].length; j++) {
				if (map[i][j] == 0) {
					isFull = false;
					break;
				}
			}
			if (isFull) {
				lines++;
				for (int m = i; m > 0; m--) {
					for (int n = 0; n < map[0].length; n++) {
						map[m][n] = map[m - 1][n];
					}
				}
			}
		}
		score += lines * lines * 10;
		if (isAccelMode) {
			up();
		}

	}

	/**
	 * 升级加速：UP
	 */
	public void up() {
		int limit = 50;
		if (score > limit * level) {
			level++;
			delay /= 1.5;
			timer.setDelay(delay);
			limit = limit * level;
		}
	}

	public void turn() {
		int temp = state;
		state = (state + 1) % 4;
		// 如果旋转后不合法，还原上一状态
		if (!check(type, state, x, y)) {
			state = temp;
		}
	}

	public void down() {
		// 如果下一个下落状态合法，则下落；不合法，则固定。
		if (check(type, state, x, y + 1)) {
			y++;
		} else {
			fix(type, state, x, y);
			clearLines();
			createShape();
		}
		this.repaint();
	}

	public void right() {
		if (check(type, state, x + 1, y)) {
			x++;
		}
	}

	public void left() {
		if (check(type, state, x - 1, y)) {
			x--;
		}
	}

	/**
	 * 绘图：重写paint()方法
	 * 
	 * @see javax.swing.JComponent#paint(java.awt.Graphics)
	 */
	@Override
	public void paint(Graphics g) {
		// 游戏区域的左、上边距
		int MARGIN_LEFT = LEN;
		int MARGIN_TOP = LEN + 10;

		// 边栏的起始坐标
		int SIDEBAR_X = LEN * 13;// 文本的横坐标
		int SIDEBAR_Y = LEN * 9; // 文本的纵坐标
		/**
		 * 填充背景色
		 */
		g.setColor(Color.white);
		g.fillRect(0, 0, (int) (this.getSize().getWidth()), (int) (this.getSize().getHeight()));

		/**
		 * 画边框
		 */
		g.setColor(Color.gray);
		for (int offset = 1; offset < 2; offset++) {
			// 绘制矩形边框：drawRect(int x,int y,int width,int height);
			g.drawRect(MARGIN_LEFT - offset, MARGIN_TOP - offset, COL * LEN + offset * 2, ROW * LEN + offset * 2);
		}
		/**
		 * 画网状线
		 */
		g.setColor(Color.gray);
		// 11条竖线
		for (int i = 0; i < 11; i++) {
			// 绘制线条：drawLine(x1,y1,x2,y2);
			g.drawLine(MARGIN_LEFT + LEN * i, MARGIN_TOP, MARGIN_LEFT + LEN * i, MARGIN_TOP + ROW * LEN);
		}
		// 21条横线
		for (int i = 0; i < 21; i++) {
			g.drawLine(MARGIN_LEFT, MARGIN_TOP + LEN * i, MARGIN_LEFT + COL * LEN, MARGIN_TOP + LEN * i);
		}
		/**
		 * 画侧栏
		 */
		// 画文本：下一个
		g.setColor(Color.gray);
		g.setFont(new Font("Times", Font.BOLD, 20));
		g.drawString("下一个：", SIDEBAR_X, LEN * 2 + 10);
		// 画提示方块（下一个方块）
		g.setColor(color[nextType]);
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				if (SHAPE[nextType][nextState][i][j] == 1) {
					// 填充3D矩形：fill3DRect(int x,int y,int width,int height,boolean raised)
					g.fill3DRect(SIDEBAR_X + 20 + j * LEN, LEN * 3 + i * LEN, LEN, LEN, true);
				}
			}
		}
		// 画文本：得分
		g.setColor(Color.gray);
		g.setFont(new Font("Times", Font.BOLD, 24));
		g.drawString("等级：" + level, SIDEBAR_X, SIDEBAR_Y);
		g.drawString("得分：" + score, SIDEBAR_X, SIDEBAR_Y + 40);
		// 画文本：游戏说明
		g.setColor(Color.gray);
		g.setFont(new Font("Times", Font.BOLD, 15));
		g.drawString("玩法：", SIDEBAR_X, SIDEBAR_Y + LEN * 4);
		g.drawString("上箭头：旋转", SIDEBAR_X, SIDEBAR_Y + LEN * 5);
		g.drawString("左箭头：左移", SIDEBAR_X, SIDEBAR_Y + LEN * 6);
		g.drawString("右箭头：右移", SIDEBAR_X, SIDEBAR_Y + LEN * 7);
		g.drawString("下箭头：下落", SIDEBAR_X, SIDEBAR_Y + LEN * 8);
		g.drawString("@栗子", SIDEBAR_X, SIDEBAR_Y + LEN * 10);

		// 画当前下落块
		g.setColor(color[type]);
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				if (SHAPE[type][state][i][j] == 1) {
					g.fill3DRect(MARGIN_LEFT + (x + j) * LEN, MARGIN_TOP + (y + i) * LEN, LEN, LEN, true);
				}
			}
		}

		// 画背景map
		for (int i = 0; i < map.length; i++) {
			for (int j = 0; j < map[0].length; j++) {
				if (map[i][j] == 1) {
					g.setColor(mapColor[i][j]);
					g.fill3DRect(MARGIN_LEFT + j * LEN, MARGIN_TOP + i * LEN, LEN, LEN, true);
				}
			}
		}

		// 画暂停
		if (isPause) {
			g.setColor(Color.black);
			g.drawString("PAUSE", MARGIN_LEFT + LEN * 8, LEN);
		}

		// 画普通模式
		if (!isAccelMode) {
			g.setColor(Color.black);
			g.drawString("Normal Mode", LEN, LEN);
		}
		// 画加速模式
		if (isAccelMode) {
			g.setColor(Color.black);
			g.drawString("Accel Mode", LEN, LEN);
		}

	}

	/**
	 * 定时器
	 */
	Timer timer = new Timer(delay, new ActionListener() {
		// 定时器任务
		@Override
		public void actionPerformed(ActionEvent e) {
			down();
		}
	});

	// 暂停
	public void setPause() {
		timer.stop();
		isPause = true;
		this.repaint();
	}

	// 继续
	public void setContinue() {
		timer.restart();
		isPause = false;
		this.repaint();
	}

	// 简单模式
	public void setNormal() {
		isAccelMode = false;
		level = 0;
		timer.setDelay(1000);
		init();
	}

	// 加速模式
	public void setAccel() {
		isAccelMode = true;
		level = 1;
		timer.setDelay(500);
		init();
	}

}
```

---



emmmmmm......不好意思，寒假作业，快到暑假了才完成。



