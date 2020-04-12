---
layout: post
title:  "recyclerview item filtering"
date:   2020-04-11 23:00:00 +0900
categories: Android
---


# 리싸이클러뷰 내 아이템 정렬

- Filterable 를 implements 후 getFilter() 메서드 오버라이드
- new Filter() 반환 간 performFiltering,publishResults 메서드 오버라이드 
- 정렬 리스트, 정렬 전 리스트, 정렬 간 리스트 필요.


## adapter

```
public class SampleAdapter extends RecyclerView.Adapter<SampleAdapter.ViewHolder> implements Filterable {

    Context context;
    ArrayList<SampleItem> unFilteredNnList;
    ArrayList<SampleItem> filteredNnList;

    public SampleAdapter(Context context, ArrayList friendList) {
        this.context = context;
        this.unFilteredNnList = friendList;
        this.filteredNnList = friendList;
    }


    @Override
    public int getItemCount() {
        return filteredNnList.size();
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup viewGroup, int i) {
        View view = LayoutInflater.from(context).inflate(R.layout.layout_nn_list_item, viewGroup, false);
        return new SampleAdapter.ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder viewHolder, int i) {
        final SampleItem item;
        item = filteredNnList.get(i);
        viewHolder.tv_Name.setText(item.getGiven_nm());       
    }


    @Override
    public Filter getFilter() {
        return new Filter() {
            @Override
            protected FilterResults performFiltering(CharSequence constraint) {

                String charString = constraint.toString();
                if (charString.isEmpty()) {
                    filteredNnList = unFilteredNnList;
                } else {
                    ArrayList<SampleItem> filteringList = new ArrayList<>();
                    for (SampleItem item : unFilteredNnList) {
                        if (item.getNickname().toLowerCase().contains(charString.toLowerCase())) {
                            filteringList.add(item);
                        }
                    }
                    filteredNnList = filteringList;
                }
                FilterResults filterResults = new FilterResults();
                filterResults.values = filteredNnList;
                return filterResults;
            }

            @Override
            protected void publishResults(CharSequence constraint, FilterResults results) {
                filteredNnList = (ArrayList<SampleItem>) results.values;
                notifyDataSetChanged();
            }
        };
    }


    public class ViewHolder extends RecyclerView.ViewHolder {

        
        TextView tv_Name;

        public ViewHolder(@NonNull View itemView) {
            super(itemView);
         
            tv_Name = itemView.findViewById(R.id.tv_Name);

        }
    }
}


```
## 모델

```
public class SampleItem {

    String nickname;

    public String getNickname() {
        return nickname;
    }

    public void setNickname(String nickname) {
        this.nickname = nickname;
    }

}

```
## 실행 시

```
friendAdapter.getFilter().filter(et_kr.getText().toString());

```