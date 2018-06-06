/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen, 
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html) 
 * */

package de.hhu.stups.sablecc.patch;

public class PositionedNode {
	private SourcePosition startPos;
	private SourcePosition endPos;

	public SourcePosition getStartPos() {
		/*
		 * Special treatment for tokens because they don't get a position in the
		 * PositionAspect
		 */
		if (this instanceof IToken) {
			final IToken token = (IToken) this;
			return new SourcePosition(token.getLine(), token.getPos());
		}

		return startPos;
	}

	public SourcePosition getEndPos() {
		/*
		 * Special treatment for tokens because they don't get a position in the
		 * PositionAspect
		 */
		if (this instanceof IToken) {
			final IToken token = (IToken) this;
			return new SourcePosition(token.getLine(), token.getPos()
					+ token.getText().length());
		}

		return endPos;
	}

	public void setStartPos(final SourcePosition startPos) {
		this.startPos = startPos;
	}

	public void setEndPos(final SourcePosition endPos) {
		this.endPos = endPos;
	}
}
