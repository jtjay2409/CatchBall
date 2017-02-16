

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseMotionListener;
import java.util.Random;

import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.Timer;

public class CatchBall extends JFrame implements MouseMotionListener,ActionListener
{
	final static byte S_WIDTH=100;
	final static byte S_HEIGHT=10;
	final static byte DIAMETER=20;
	final static short SY=400;
	int sx=300;
	int bx,by,score;
	Timer timer;
	boolean right,down,lost;
	byte dx,dy,ball_left;
	Font f;
	public CatchBall()
	{
		ball_left=4;
		f=new Font("Arial", Font.BOLD, 30);
		score=0;
		dx=increment();
		dy=increment();
		timer=new Timer(40, this);
		addMouseMotionListener(this);
		setSize(600, 500);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		setLocationRelativeTo(null);
		setResizable(false);
		setVisible(true);
		initialize();
		timer.start();
	}
	
	byte increment()
	{
		Random r=new Random();
		byte i;
		do{
			i=(byte) r.nextInt(8);
		}while(i<4);
		return i;
	}
	
	@Override
	public void paint(Graphics arg0)
	{
		arg0.clearRect(0, 0, getWidth(), getHeight());
		arg0.setColor(Color.blue);
		arg0.fillOval(bx, by, DIAMETER, DIAMETER);
		arg0.setColor(Color.green);
		arg0.fillRoundRect(sx, SY, S_WIDTH, S_HEIGHT, 10, 5);
		arg0.setFont(f);
		arg0.setColor(Color.red);
		arg0.drawString("Score : "+score, 410, 70);
	}
	public static void main(String[] args)
	{
		new CatchBall();
	}

	@Override
	public void mouseDragged(MouseEvent arg0) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseMoved(MouseEvent arg0)
	{
		sx=arg0.getX();
		if(sx>getWidth()-S_WIDTH)
			sx=getWidth()-S_WIDTH;
		repaint();
	}
	void initialize()
	{
		lost=false;
		by=20;
		Random r=new Random();
		do
		{
		bx=r.nextInt(getWidth()-50);
		}while(bx<50);
		right=down=true;
	}

	@Override
	public void actionPerformed(ActionEvent arg0)
	{
		if(by>=getHeight()+10)
		{
			ball_left--;
			if(ball_left==0)
			{
				timer.stop();
				JOptionPane.showMessageDialog(this, "Game Over");
				this.dispose();
			}
			else
			{
				JOptionPane.showMessageDialog(this, ball_left+" Ball left");
			}
			initialize();
		}
		if(by+DIAMETER>=SY && !lost)
		{
			if(bx>=sx-DIAMETER/2 && bx<=sx+S_WIDTH-DIAMETER/2)
			{
				score=score+2;
				down=false;
				dy=increment();
			}
			else
				lost=true;
		}
		if(by<=20)
		{
			down=true;
			dy=increment();
		}
		
		if(bx+DIAMETER>=getWidth())
		{
			right=false;
			dx=increment();
		}
		if(bx<=0)
		{
			right=true;
			dx=increment();
		}
		if(down)
			by=by+dy;
		else
			by=by-dy;
		if(right)
			bx=bx+dx;
		else
			bx=bx-dx;
		repaint();
	}
}
