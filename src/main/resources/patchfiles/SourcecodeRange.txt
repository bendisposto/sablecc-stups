/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen, 
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html) 
 * */

package de.hhu.stups.sablecc.patch;

public class SourcecodeRange {
	private final int beginIndex;
	private final int endIndex;

	public SourcecodeRange(final int beginIndex, final int endIndex) {
		this.beginIndex = beginIndex;
		this.endIndex = endIndex;
	}

	public int getBeginIndex() {
		return beginIndex;
	}

	public int getEndIndex() {
		return endIndex;
	}

	@Override
	public String toString() {
		return "(" + beginIndex + "/" + endIndex + ")";
	}
}
