/*
 * (c) 2009 Lehrstuhl fuer Softwaretechnik und Programmiersprachen, 
 * Heinrich Heine Universitaet Duesseldorf
 * This software is licenced under EPL 1.0 (http://www.eclipse.org/org/documents/epl-v10.html) 
 * */

package de.hhu.stups.sablecc.patch;

import java.util.Map;


public interface IParser {
	public Map<PositionedNode, SourcecodeRange> getMapping();
}
