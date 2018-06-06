/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen, 
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html) 
 * */

package de.hhu.stups.sablecc.patch;

public class SourcePosition implements Comparable<SourcePosition>  {

	private final int line;

	private final int pos;

	public SourcePosition(final int line, final int pos) {
		this.line = line;
		this.pos = pos;
	}

	public int getLine() {
		return line;
	}

	public int getPos() {
		return pos;
	}
	
	public int compareTo(SourcePosition that) {
		if (that.line < line) return 1;
		if (that.line > line) return -1;
		return pos-that.pos;
	}
	
	@Override
	public String toString() {
		return "(" + line + "," + pos + ")";
	}
	
	
}
