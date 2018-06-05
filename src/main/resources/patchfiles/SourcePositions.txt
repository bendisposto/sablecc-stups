/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen,
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html)
 * */

package de.hhu.stups.sablecc.patch;

import java.util.Iterator;
import java.util.List;
import java.util.Map;

public class SourcePositions {
	private final List<IToken> tokenList;
	private final Map<PositionedNode, SourcecodeRange> positions;

	public SourcePositions(final List<IToken> tokenList,
			final Map<PositionedNode, SourcecodeRange> positions) {
		this.tokenList = tokenList;
		this.positions = positions;
	}

	/**
	 * Returns the {@link SourcecodeRange} of this {@link PositionedNode} or
	 * {@code null} if no {@link SourcecodeRange} is available.
	 *
	 * @param node the node with source code information
	 * @return the source code range of {@code node}
	 */
	public SourcecodeRange getSourcecodeRange(final PositionedNode node) {
		return positions.get(node);
	}

	/**
	 * Returns the line in which this {@link PositionedNode} begins. The value
	 * {@code 0} is returned if no sourcecode range is available for this
	 * {@link PositionedNode}.
	 *
	 * @param node the node with source code information
	 * @return the line where the {@code node} starts
	 */
	public int getBeginLine(final PositionedNode node) {
		if (node instanceof IToken) {
			return ((IToken) node).getLine();
		}

		return getBeginLine(getSourcecodeRange(node));
	}

	public int getBeginLine(final SourcecodeRange range) {
		if (range != null) {
			return tokenList.get(range.getBeginIndex()).getLine();
		} else {
			return 0;
		}
	}

	/**
	 * Returns the column of the first character of this {@link PositionedNode},
	 * i.e. the begin column. The value {@code 0} is returned if no
	 * sourcecode range is available for this {@link PositionedNode}.
	 *
	 * @param node	the node with source code information
	 * @return	the begin column of {@code node}
	 */
	public int getBeginColumn(final PositionedNode node) {
		if (node instanceof IToken) {
			return ((IToken) node).getPos();
		}

		return getBeginColumn(getSourcecodeRange(node));
	}

	public int getBeginColumn(final SourcecodeRange range) {
		if (range != null) {
			return tokenList.get(range.getBeginIndex()).getPos();
		} else {
			return 0;
		}
	}

	/**
	 * Returns the line in which the {@link PositionedNode} ends. The value
	 * {@code 0} is returned if no sourcecode range is available for this
	 * {@link PositionedNode}.
	 *
	 * @param node	the node with source code information
	 * @return	the end line of <code>node</code>
	 */
	public int getEndLine(final PositionedNode node) {
		// TODO handle multi line comments
		/*
		 * if (node instanceof TComment) { final TComment comment = (TComment)
		 * node; return comment.getLine() + countLineBreaks(comment); }
		 */

		if (node instanceof IToken) {
			return ((IToken) node).getLine();
		}

		return getEndLine(getSourcecodeRange(node));
	}

	private int getEndLine(final SourcecodeRange range) {
		if (range != null) {
			return tokenList.get(range.getEndIndex()).getLine();
		} else {
			return 0;
		}
	}

	// private int countLineBreaks(final PositionedNode node) {
	// final char[] text = getNodeString(node).toCharArray();
	// int count = 0;
	//
	// for (int i = 0; i < text.length; i++) {
	// if (text[i] == '\n' || text[i] == '\r') {
	// count++;
	// }
	// }
	//
	// return count;
	// }

	/**
	 * Returns the last column of this {@link PositionedNode}, i.e. the column
	 * of the last character of the {@link PositionedNode}. The value
	 * {@code 0} is returned if no sourcecode range is available for this
	 * {@link PositionedNode}.
	 *
	 * @param node	the node with source code information
	 * @return	the end column of <code>node</code>
	 */
	public int getEndColumn(final PositionedNode node) {
		// TODO handle multi line comments
		/*
		 * if (node instanceof TComment) { return getEndColumn((TComment) node);
		 * }
		 */

		if (node instanceof IToken) {
			final IToken token = (IToken) node;
			return token.getPos() + token.getText().length() - 1;
		}

		return getEndColumn(getSourcecodeRange(node));
	}

	private int getEndColumn(final SourcecodeRange range) {
		if (range != null) {
			final IToken token = tokenList.get(range.getEndIndex());
			return token.getPos() + token.getText().length() - 1;
		} else {
			return 0;
		}
	}

	/*
	 * private int getEndColumn(final TComment commentToken) { final String
	 * asString = commentToken.getText(); final StringTokenizer tokenizer = new
	 * StringTokenizer(asString, "\n\r");
	 *
	 * // multi line comment if (tokenizer.countTokens() > 1) { String line =
	 * null; while (tokenizer.hasMoreTokens()) { line = tokenizer.nextToken(); }
	 *
	 * if (line == null) { return 0; }
	 *
	 * return line.length(); } // single line comment else { return
	 * commentToken.getPos() + asString.length(); } }
	 */

	/**
	 * Returns the array of {@link IToken}s belonging to this
	 * {@link PositionedNode}. The array may be empty, if no sourcecode range
	 * can be determined for this {@link PositionedNode}.
	 *
	 * @param node	the node with source code information
	 * @return	all tokens of {@code node}
	 */
	public IToken[] getTokens(final PositionedNode node) {
		if (node instanceof IToken) {
			return new IToken[] { (IToken) node };
		}

		final SourcecodeRange range = getSourcecodeRange(node);

		if (range != null) {
			final int beginIndex = range.getBeginIndex();
			final int endIndex = range.getEndIndex();
			final IToken[] result = new IToken[endIndex - beginIndex + 1];

			for (int i = 0; i + beginIndex <= endIndex; i++) {
				result[i] = tokenList.get(i + beginIndex);
			}

			return result;
		} else {
			return new IToken[0];
		}
	}

	public String getNodeString(final PositionedNode node) {
		// TODO handle comments
		/*
		 * if (node instanceof TComment) { return ((TComment) node).getText(); }
		 */

		return getRangeString(getSourcecodeRange(node));
	}

	public String getRangeString(final SourcecodeRange range) {
		final StringBuilder buffer = new StringBuilder();

		if (range != null) {
			final int beginIndex = range.getBeginIndex();
			final int endIndex = range.getEndIndex();

			for (int i = beginIndex; i <= endIndex; i++) {
				buffer.append(tokenList.get(i).getText());
			}
		}

		return buffer.toString();
	}

	// TODO handle comments
	/*
	 *
	 * public IToken getCommentBefore(final PositionedNode node) { final
	 * SourcecodeRange range = getSourcecodeRange(node); final int beginIndex =
	 * range.getBeginIndex(); if (beginIndex > 0) { final IToken token =
	 * tokenList.get(beginIndex - 1); if (token instanceof TComment) { return
	 * token; } else { return null; } } else { return null; } }
	 *
	 * public IToken getCommentAfter(final PositionedNode node) { final
	 * SourcecodeRange range = getSourcecodeRange(node); final int endIndex =
	 * range.getEndIndex(); if (endIndex < tokenList.size() - 1) { final IToken
	 * token = tokenList.get(endIndex + 1); if (token instanceof TComment) {
	 * return token; } else { return null; } } else { return null; } } public
	 * IToken[] getIncludedComments(final PositionedNode node) { final
	 * SourcecodeRange range = getSourcecodeRange(node);
	 *
	 * if (range != null) { final int beginIndex = range.getBeginIndex(); final
	 * int endIndex = range.getEndIndex(); final List<IToken> comments = new
	 * ArrayList<IToken>();
	 *
	 * for (int i = 0; i + beginIndex <= endIndex; i++) { final IToken token =
	 * tokenList.get(i + beginIndex); if (token instanceof TComment) {
	 * comments.add(token); } }
	 *
	 * return comments.toArray(new IToken[comments.size()]); } else { return new
	 * IToken[0]; } }
	 */

	public List<IToken> getTokenList() {
		return tokenList;
	}

	public void replaceMapping(final PositionedNode origNode,
			final PositionedNode newNode) {
		final SourcecodeRange sourcecodeRange = positions.remove(origNode);

		if (sourcecodeRange != null) {
			positions.put(newNode, sourcecodeRange);
		}
	}

	public PositionedNode getSurroundingNode(final int index) {
		if (index < 0 || index >= tokenList.size()) {
			return null;
		}

		PositionedNode bestNode = null;
		int bestBeginIndex = 0;
		int bestEndIndex = tokenList.size() - 1;

		// TODO find better solution than searching all?
		for (final Iterator<PositionedNode> iterator = positions.keySet()
				.iterator(); iterator.hasNext();) {
			final PositionedNode node = iterator.next();
			final SourcecodeRange range = positions.get(node);

			final int beginIndex = range.getBeginIndex();
			final int endIndex = range.getEndIndex();

			if (beginIndex <= index && endIndex >= index
					&& beginIndex >= bestBeginIndex && endIndex <= bestEndIndex) {
				bestNode = node;
				bestBeginIndex = beginIndex;
				bestEndIndex = endIndex;
			}
		}

		return bestNode;
	}

	/**
	 * <p>
	 * Finds the index of the token that belongs to the position.
	 * </p>
	 * <p>
	 * If no token list is available {@code -1} is returned. For
	 * {@code line < 0} the index {@code 0} is returned. If
	 * {@code line >= 1 && column < 0} the line number is returned.
	 * </p>
	 * <p>
	 * If the line matches but the requested column is beyond the max. length of
	 * the line, the last token of this line is returned. The last token of all
	 * tokens (EOF) is chosen if the requested positions is beyond the
	 * absolutely last token.
	 * </p>
	 * <p>
	 * <b>Attention</b>: Line and column counting starts at 1!
	 * </p>
	 *
	 * @param line
	 *            line of the position
	 * @param column
	 *            column of the position
	 * @return Index in {@link #tokenList}
	 */
	public int getTokenforPosition(final int line, final int column) {
		// Sort out nonsense input
		if (tokenList.size() == 0) {
			return -1;
		}
		if (line < 1) {
			return 0;
		}
		if (column < 1) {
			return line;
		}

		/*
		 * Shortcut for special case: Position beyond last token
		 */
		final IToken lastToken = tokenList.get(tokenList.size() - 1);
		if (line > lastToken.getLine()
				|| (line == lastToken.getLine() && column > lastToken.getPos()
						+ lastToken.getText().length())) {
			return tokenList.size() - 1;
		}

		int result = -1;
		int left = 0;
		int right = tokenList.size() - 1;

		while (left <= right && result < 0) {
			if (left != right) {
				final int currentIndex = left + (right - left) / 2;

				final int lineDiff = line
						- tokenList.get(currentIndex).getLine();

				if (lineDiff > 0) {
					// continue in right half
					left = Math.min(currentIndex + 1, right);
				} else if (lineDiff < 0) {
					// continue in left half
					right = Math.max(currentIndex - 1, left);
				} else {
					// we are in the correct line now, switch to linear search
					IToken token = tokenList.get(currentIndex);
					result = currentIndex;

					final int compare = compareTokenColumn(token, column);

					// move left
					if (compare < 0) {
						while (compareTokenColumn(token, column) < 0) {
							result--;

							if (result < 0) {
								break;
							}

							token = tokenList.get(result);
						}
					}
					// move right
					else if (compare > 0) {
						while (compareTokenColumn(token, column) > 0) {
							result++;

							if (result > tokenList.size() - 1) {
								result = tokenList.size() - 1;
								break;
							}

							token = tokenList.get(result);

							/*
							 * Only move as long as line end is not reached.
							 * This happens when queried for column beyond max
							 * column of this line. Then we return the index of
							 * the last token in this line.
							 */
							if (token.getLine() > line) {
								result--;
								break;
							}
						}
					}
				}
			} else {
				result = left;
			}
		}

		return result;
	}

	/**
	 * Compares the token position (within line only) with the column.
	 *
	 * @return {@code -1} if {@code column < beginColumn},
	 *         {@code 1} if {@code endColumn < column} or
	 *         {@code 0} if the column is within the range of the token.
	 */
	private int compareTokenColumn(final IToken token, final int column) {
		final int beginColumn = token.getPos();
		final int endColumn = beginColumn + token.getText().length() - 1;

		if (column < beginColumn) {
			return -1;
		} else if (endColumn < column) {
			return 1;
		} else {
			return 0;
		}
	}
}
