Classification:
	Binary:
		True Positives(TP):
			Number of True cases identified as True
		False Positives(FP) 
			Number of False cases identified as True
		True Negatives(TN):
			Number of False cases identified as False
		False Negatives(FN) 
			Number of False cases identified as False
			
|          | Predicted True | Predicted False |
| -------- | -------------- | --------------- |
| Is True  | TP             | FN              |
| Is False | FP             | TN              |
		Precision:
			Percentage of Predicted Trues that are true: $\frac {TP}{TP + FP}$
			Normalized by column.
		Recall:
			Percentage of Trues that are Predicted True: $\frac {TP}{TP + FN}$
			Normalized by row.
	Multiclass:
		Confusion Matrix
			$M{i}{j}$ Number of True i's predicted as j's
			Substract the diagonal and you get all the incorrect predictions, you can normalize by row or by column.
			![[Pasted image 20231129194402.png]]
			By row:
				![[Pasted image 20231129194057.png]]
				Percentage of the wrongly predicted True 0's that were predicted as 8's 
			By column:
				![[Pasted image 20231129194146.png]]
				Percentage of the digits wrongly predicted as 7's that were actually 9's




