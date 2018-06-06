/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen, 
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html) 
 * */

package de.hhu.stups.sablecc.patch;


public interface IToken {

	public String getText();

	public void setText(String text);

	public int getLine();

	public void setLine(int line);

	public int getPos();

	public void setPos(int pos);
}
